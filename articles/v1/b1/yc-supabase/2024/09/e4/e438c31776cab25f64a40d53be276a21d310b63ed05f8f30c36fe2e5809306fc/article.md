---
schema_version: "1.0.0"
document_id: "e438c31776cab25f64a40d53be276a21d310b63ed05f8f30c36fe2e5809306fc"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/flutter-uber-clone"
published_at: "2024-09-05T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T22:01:06.812214+00:00"
content_hash: "sha256:2fe62014aacbd08de7dd503ae542c11125d85e758adee673c5ed6c2bf060192c"
---

# Building an Uber Clone with Flutter and Supabase

[Do you prefer audio-visual learning? Watch the video guide!](https://youtu.be/cL4pVpaOH9o)


[Or jump straight into the code](https://github.com/dshukertjr/uber-clone)


Postgres can handle geography data efficiently thanks to the PostGIS extension. Combining it with Supabase realtime and you can create a real-time location tracking app.


In this tutorial, we will guide you through the process of creating an Uber-like application using Flutter and Supabase. This project demonstrates the capabilities of Supabase for building complex, real-time applications with minimal backend code.


## App Overview#


An actual Uber app has two apps, the consumer facing app and the driver facing app. This article only covers the consumer facing app. The app works by first choosing a destination, and then waiting for the driver to come pick them up. Once they are picked up, they head to the destination and the journey is complete once they arrive at the destination. Throughout the lifecycle of the app, the driver’s position is shared on screen in real-time.


The focus of the app is to showcase how to use Supabase realtime with geographical data, so handling payments will not be covered in this article.


## Prerequisites#


Before beginning, ensure you have:


1. Flutter installed
2. A Supabase account - head to[database.new](http://database.new/) if you don’t have one yet.
3. Basic knowledge of Dart and Flutter


## Step 1: Project Setup#


Start by creating a blank Flutter project.


`
_10


flutter create canvas --empty --platforms=ios,android


`


Then, add the required dependencies to your` pubspec.yaml` file:


`
_10


supabase_flutter: ^2.5.9


_10


google_maps_flutter: ^2.7.0


_10


geolocator: ^12.0.0


_10


duration: ^3.0.13


_10


intl: ^0.19.0


`


` google_maps_flutter` is used to display the map on our app. We will also draw and move icons on the map.` geolocator` is used to access the GPS information.` duration` is used to parse duration value returned from Google’s routes API, and` intl` is used to display currencies nicely.


In addition to adding it to` pubspec.yaml` file,` google_maps_flutter` requires additional setup to get started. Follow the[readme.md](https://pub.dev/packages/google_maps_flutter) file to configure Google Maps for the platform you want to support.


Run` flutter pub get` to install these dependencies.


## Step 2: Supabase Initialization#


In your` main.dart` file, initialize Supabase with the following code:


`
_11


import 'package:supabase_flutter/supabase_flutter.dart';


_11


_11


void main() async {


_11


await Supabase.initialize(


_11


url: 'YOUR_SUPABASE_URL',


_11


anonKey: 'YOUR_SUPABASE_ANON_KEY',


_11


);


_11


runApp(const MainApp());


_11


}


_11


_11


final supabase = Supabase.instance.client;


`


Replace` YOUR_SUPABASE_URL` and` YOUR_SUPABASE_ANON_KEY` with your actual Supabase project credentials.


## Step 3: Database Configuration#


We need to create two tables for this application. The` drivers` table holds the vehicle information as well as the position. Notice that we have a` latitude` and` longitude` generated column. These columns are generated from the` location` column, and will be used to display the real-time location on the map later on.


The` rides` table holds information about customer’s request to get a ride.


`
_24


-- Enable the "postgis" extension


_24


create extension postgis with schema extensions;


_24


_24


create table if not exists public.drivers (


_24


id uuid primary key default gen_random_uuid(),


_24


model text not null,


_24


number text not null,


_24


is_available boolean not null default false,


_24


location geography(POINT) not null,


_24


latitude double precision generated always as (st_y(location::geometry)) stored,


_24


longitude double precision generated always as (st_x(location::geometry)) stored


_24


);


_24


_24


create type ride_status as enum ('picking_up', 'riding', 'completed');


_24


_24


create table if not exists public.rides (


_24


id uuid primary key default gen_random_uuid(),


_24


driver_id uuid not null references public.drivers(id),


_24


passenger_id uuid not null references auth.users(id),


_24


origin geography(POINT) not null,


_24


destination geography(POINT) not null,


_24


fare integer not null,


_24


status ride_status not null default 'picking_up'


_24


);


`


Let’s also set[row level security](https://supabase.com/docs/guides/database/postgres/row-level-security) policies for the tables to secure our database.


`
_10


alter table public.drivers enable row level security;


_10


create policy "Any authenticated users can select drivers." on public.drivers for select to authenticated using (true);


_10


create policy "Drivers can update their own status." on public.drivers for update to authenticated using (auth.uid() = id);


_10


_10


alter table public.rides enable row level security;


_10


create policy "The driver or the passenger can select the ride." on public.rides for select to authenticated using (driver_id = auth.uid() or passenger_id = auth.uid());


_10


create policy "The driver can update the status. " on public.rides for update to authenticated using (auth.uid() = driver_id);


`


Lastly, we will create a few database functions and triggers. The first function and trigger updates the driver status depending on the status of the ride. This ensures that the driver status is always in sync with the status of the ride.


The second function is for the customer to find available drivers. This function will be called from the Flutter app, which automatically find available drivers within 3,000m radius and returns the driver ID and a newly created ride ID if a driver was found.


`
_52


-- Create a trigger to update the driver status


_52


create function update_driver_status()


_52


returns trigger


_52


language plpgsql


_52


as $$


_52


begin


_52


if new.status = 'completed' then


_52


update public.drivers


_52


set is_available = true


_52


where id = new.driver_id;


_52


else


_52


update public.drivers


_52


set is_available = false


_52


where id = new.driver_id;


_52


end if;


_52


return new;


_52


end $$;


_52


_52


create trigger driver_status_update_trigger


_52


after insert or update on rides


_52


for each row


_52


execute function update_driver_status();


_52


_52


-- Finds the closest available driver within 3000m radius


_52


create function public.find_driver(origin geography(POINT), destination geography(POINT), fare int)


_52


returns table(driver_id uuid, ride_id uuid)


_52


language plpgsql


_52


as $$


_52


declare


_52


v_driver_id uuid;


_52


v_ride_id uuid;


_52


begin


_52


select


_52


drivers.id into v_driver_id


_52


from public.drivers


_52


where is_available = true


_52


and st_dwithin(origin, location, 3000)


_52


order by drivers.location <-> origin


_52


limit 1;


_52


_52


-- return null if no available driver is found


_52


if v_driver_id is null then


_52


return;


_52


end if;


_52


_52


insert into public.rides (driver_id, passenger_id, origin, destination, fare)


_52


values (v_driver_id, auth.uid(), origin, destination, fare)


_52


returning id into v_ride_id;


_52


_52


return query


_52


select v_driver_id as driver_id, v_ride_id as ride_id;


_52


end $$ security definer;


`


## Step 4: Defining the models#


Start by defining the models for this app. The` AppState` enum holds the 5 different state that this app could take in the order that it proceeds. The` Ride` and` Driver` class are simple data class for the` rides` and` drivers` table we created earlier.


`
_66


enum AppState {


_66


choosingLocation,


_66


confirmingFare,


_66


waitingForPickup,


_66


riding,


_66


postRide,


_66


}


_66


_66


enum RideStatus {


_66


picking_up,


_66


riding,


_66


completed,


_66


}


_66


_66


class Ride {


_66


final String id;


_66


final String driverId;


_66


final String passengerId;


_66


final int fare;


_66


final RideStatus status;


_66


_66


Ride({


_66


required this.id,


_66


required this.driverId,


_66


required this.passengerId,


_66


required this.fare,


_66


required this.status,


_66


});


_66


_66


factory Ride.fromJson(Map<String, dynamic> json) {


_66


return Ride(


_66


id: json\['id'\],


_66


driverId: json\['driver_id'\],


_66


passengerId: json\['passenger_id'\],


_66


fare: json\['fare'\],


_66


status: RideStatus.values


_66


.firstWhere((e) => e.toString().split('.').last == json\['status'\]),


_66


);


_66


}


_66


}


_66


_66


class Driver {


_66


final String id;


_66


final String model;


_66


final String number;


_66


final bool isAvailable;


_66


final LatLng location;


_66


_66


Driver({


_66


required this.id,


_66


required this.model,


_66


required this.number,


_66


required this.isAvailable,


_66


required this.location,


_66


});


_66


_66


factory Driver.fromJson(Map<String, dynamic> json) {


_66


return Driver(


_66


id: json\['id'\],


_66


model: json\['model'\],


_66


number: json\['number'\],


_66


isAvailable: json\['is_available'\],


_66


location: LatLng(json\['latitude'\], json\['longitude'\]),


_66


);


_66


}


_66


}


`


## Step 5: Main UI Implementation#


Create a` UberCloneMainScreen` widget to serve as the main interface for the application. This widget will manage the five different` AppState` that we created in the previous step.


1. Location selection - The customer scrolls through the map and chooses the destination
2. Fare confirmation - The fare is displayed to the user, and the customer can accept the fare to find a nearby driver
3. Pickup waiting - A driver was found, and the customer is waiting for the driver to arrive
4. In-ride - The customer has got on the car, and they are headed to the destination
5. Post-ride - The customer has arrived at the destination, and a thank you modal is displayed


For statuses 3, 4, and 5, the status update happens on the driver’s app, which we don’t have. So you can directly modify the data from the Supabase dashboard and update the status of the ride.


`
_152


class UberCloneMainScreen extends StatefulWidget {


_152


const UberCloneMainScreen({super.key});


_152


_152


@override


_152


UberCloneMainScreenState createState() => UberCloneMainScreenState();


_152


}


_152


_152


class UberCloneMainScreenState extends State<UberCloneMainScreen> {


_152


AppState _appState = AppState.choosingLocation;


_152


GoogleMapController? _mapController;


_152


_152


/// The default camera position is arbitrarily set to San Francisco.


_152


CameraPosition _initialCameraPosition = const CameraPosition(


_152


target: LatLng(37.7749, -122.4194),


_152


zoom: 14.0,


_152


);


_152


_152


/// The selected destination by the user.


_152


LatLng? _selectedDestination;


_152


_152


/// The current location of the user.


_152


LatLng? _currentLocation;


_152


_152


final Set<Polyline> _polylines = {};


_152


final Set<Marker> _markers = {};


_152


_152


/// Fare in cents


_152


int? _fare;


_152


StreamSubscription<dynamic>? _driverSubscription;


_152


StreamSubscription<dynamic>? _rideSubscription;


_152


Driver? _driver;


_152


_152


LatLng? _previousDriverLocation;


_152


BitmapDescriptor? _pinIcon;


_152


BitmapDescriptor? _carIcon;


_152


_152


@override


_152


void initState() {


_152


super.initState();


_152


_signInIfNotSignedIn();


_152


_checkLocationPermission();


_152


_loadIcons();


_152


}


_152


_152


@override


_152


void dispose() {


_152


_cancelSubscriptions();


_152


super.dispose();


_152


}


_152


_152


// TODO: Add missing methods


_152


_152


@override


_152


Widget build(BuildContext context) {


_152


return Scaffold(


_152


appBar: AppBar(


_152


title: Text(_getAppBarTitle()),


_152


),


_152


body: Stack(


_152


children: \[


_152


_currentLocation == null


_152


? const Center(child: CircularProgressIndicator())


_152


: GoogleMap(


_152


initialCameraPosition: _initialCameraPosition,


_152


onMapCreated: (GoogleMapController controller) {


_152


_mapController = controller;


_152


},


_152


myLocationEnabled: true,


_152


onCameraMove: _onCameraMove,


_152


polylines: _polylines,


_152


markers: _markers,


_152


),


_152


if (_appState == AppState.choosingLocation)


_152


Center(


_152


child: Image.asset(


_152


'assets/images/center-pin.png',


_152


width: 96,


_152


height: 96,


_152


),


_152


),


_152


\],


_152


),


_152


floatingActionButton: _appState == AppState.choosingLocation


_152


? FloatingActionButton.extended(


_152


onPressed: _confirmLocation,


_152


label: const Text('Confirm Destination'),


_152


icon: const Icon(Icons.check),


_152


)


_152


: null,


_152


floatingActionButtonLocation: FloatingActionButtonLocation.centerFloat,


_152


bottomSheet: _appState == AppState.confirmingFare ||


_152


_appState == AppState.waitingForPickup


_152


? Container(


_152


width: MediaQuery.of(context).size.width,


_152


padding: const EdgeInsets.all(16)


_152


.copyWith(bottom: 16 + MediaQuery.of(context).padding.bottom),


_152


decoration: BoxDecoration(


_152


color: Colors.white,


_152


boxShadow: \[


_152


BoxShadow(


_152


color: Colors.grey.withOpacity(0.5),


_152


spreadRadius: 5,


_152


blurRadius: 7,


_152


offset: const Offset(0, 3),


_152


),


_152


\],


_152


),


_152


child: Column(


_152


mainAxisSize: MainAxisSize.min,


_152


children: \[


_152


if (_appState == AppState.confirmingFare) ...\[


_152


Text('Confirm Fare',


_152


style: Theme.of(context).textTheme.titleLarge),


_152


const SizedBox(height: 16),


_152


Text(


_152


'Estimated fare: ${NumberFormat.currency(


_152


symbol:


_152


'\\$', // You can change this to your preferred currency symbol


_152


decimalDigits: 2,


_152


).format(_fare! / 100)}',


_152


style: Theme.of(context).textTheme.titleMedium),


_152


const SizedBox(height: 16),


_152


ElevatedButton(


_152


onPressed: _findDriver,


_152


style: ElevatedButton.styleFrom(


_152


minimumSize: const Size(double.infinity, 50),


_152


),


_152


child: const Text('Confirm Fare'),


_152


),


_152


\],


_152


if (_appState == AppState.waitingForPickup &&


_152


_driver != null) ...\[


_152


Text('Your Driver',


_152


style: Theme.of(context).textTheme.titleLarge),


_152


const SizedBox(height: 8),


_152


Text('Car: ${_driver!.model}',


_152


style: Theme.of(context).textTheme.titleMedium),


_152


const SizedBox(height: 8),


_152


Text('Plate Number: ${_driver!.number}',


_152


style: Theme.of(context).textTheme.titleMedium),


_152


const SizedBox(height: 16),


_152


Text(


_152


'Your driver is on the way. Please wait at the pickup location.',


_152


style: Theme.of(context).textTheme.bodyMedium),


_152


\]


_152


\],


_152


),


_152


)


_152


: const SizedBox.shrink(),


_152


);


_152


}


_152


}


`


The code above still has many missing methods, so do not worry if you see many errors.


## Step 6: Location Selection Implementation#


The way the customer chooses the destination is by scrolling through the map and tapping on the confirmation FAB. Once the FAB is pressed, the` _confirmLocation` method is called, which calls a Supabase Edge Function called` route` . This` route` function returns a list of coordinates to create a polyline to get from the current location to the destination. We then draw the polyline on the Google Maps to provide to simulate an Uber-like user experience.


`
_72


Future<void> _confirmLocation() async {


_72


if (_selectedDestination != null && _currentLocation != null) {


_72


try {


_72


final response = await supabase.functions.invoke(


_72


'route',


_72


body: {


_72


'origin': {


_72


'latitude': _currentLocation!.latitude,


_72


'longitude': _currentLocation!.longitude,


_72


},


_72


'destination': {


_72


'latitude': _selectedDestination!.latitude,


_72


'longitude': _selectedDestination!.longitude,


_72


},


_72


},


_72


);


_72


_72


final data = response.data as Map<String, dynamic>;


_72


final coordinates = data\['legs'\]\[0\]\['polyline'\]\['geoJsonLinestring'\]


_72


\['coordinates'\] as List<dynamic>;


_72


final duration = parseDuration(data\['duration'\] as String);


_72


_fare = ((duration.inMinutes * 40)).ceil();


_72


_72


final List<LatLng> polylineCoordinates = coordinates.map((coord) {


_72


return LatLng(coord\[1\], coord\[0\]);


_72


}).toList();


_72


_72


setState(() {


_72


_polylines.add(Polyline(


_72


polylineId: const PolylineId('route'),


_72


points: polylineCoordinates,


_72


color: Colors.black,


_72


width: 5,


_72


));


_72


_72


_markers.add(Marker(


_72


markerId: const MarkerId('destination'),


_72


position: _selectedDestination!,


_72


icon: _pinIcon ??


_72


BitmapDescriptor.defaultMarkerWithHue(BitmapDescriptor.hueRed),


_72


));


_72


});


_72


_72


LatLngBounds bounds = LatLngBounds(


_72


southwest: LatLng(


_72


polylineCoordinates


_72


.map((e) => e.latitude)


_72


.reduce((a, b) => a < b ? a : b),


_72


polylineCoordinates


_72


.map((e) => e.longitude)


_72


.reduce((a, b) => a < b ? a : b),


_72


),


_72


northeast: LatLng(


_72


polylineCoordinates


_72


.map((e) => e.latitude)


_72


.reduce((a, b) => a > b ? a : b),


_72


polylineCoordinates


_72


.map((e) => e.longitude)


_72


.reduce((a, b) => a > b ? a : b),


_72


),


_72


);


_72


_mapController?.animateCamera(CameraUpdate.newLatLngBounds(bounds, 50));


_72


_goToNextState();


_72


} catch (e) {


_72


if (mounted) {


_72


ScaffoldMessenger.of(context).showSnackBar(


_72


SnackBar(content: Text('Error: ${e.toString()}')),


_72


);


_72


}


_72


}


_72


}


_72


}


`


Let’s also create the` route` edge functions. This function calls the[routes API from Google](https://developers.google.com/maps/documentation/routes) , which provides us the array of lines on the map to take us from the customer’s current location to the destination.


Run the following commands to create the edge functions.


`
_10


# initialize Supabase


_10


npx supabase init


_10


_10


# Create a new function named route


_10


npx supabase functions new route


`


`
_46


type Coordinates = {


_46


latitude: number


_46


longitude: number


_46


}


_46


_46


Deno.serve(async (req) => {


_46


const {


_46


origin,


_46


destination,


_46


}: {


_46


origin: Coordinates


_46


destination: Coordinates


_46


} = await req.json()


_46


_46


const response = await fetch(


_46


\`https://routes.googleapis.com/directions/v2:computeRoutes?key=${Deno.env.get(


_46


'GOOGLE_MAPS_API_KEY'


_46


)}\`,


_46


{


_46


method: 'POST',


_46


headers: {


_46


'Content-Type': 'application/json',


_46


'X-Goog-FieldMask':


_46


'routes.duration,routes.distanceMeters,routes.polyline,routes.legs.polyline',


_46


},


_46


body: JSON.stringify({


_46


origin: { location: { latLng: origin } },


_46


destination: { location: { latLng: destination } },


_46


travelMode: 'DRIVE',


_46


polylineEncoding: 'GEO_JSON_LINESTRING',


_46


}),


_46


}


_46


)


_46


_46


if (!response.ok) {


_46


const error = await response.json()


_46


console.error({ error })


_46


throw new Error(\`HTTP error! status: ${response.status}\`)


_46


}


_46


_46


const data = await response.json()


_46


_46


const res = data.routes\[0\]


_46


_46


return new Response(JSON.stringify(res), { headers: { 'Content-Type': 'application/json' } })


_46


})


`


Once the function is ready, you can[run it locally](https://supabase.com/docs/guides/functions/quickstart) or[deploy it to a remote Supabase instance](https://supabase.com/docs/guides/functions/deploy) .


## Step 7: Driver Assignment#


Now, once a route is displayed on the map and the customer agrees on the fare, a driver needs to be found. We created a convenient method for this earlier, so we can just call the method to find a driver and create a new ride.


If a driver was successfully found, we listen to real-time changes on both the driver and the ride to keep track of the driver’s position and the ride’s current status. For this, we use the[.stream()](https://supabase.com/docs/reference/dart/stream) method.


`
_72


/// Finds a nearby driver


_72


///


_72


/// When a driver is found, it subscribes to the driver's location and ride status.


_72


Future<void> _findDriver() async {


_72


try {


_72


final response = await supabase.rpc('find_driver', params: {


_72


'origin':


_72


'POINT(${_currentLocation!.longitude} ${_currentLocation!.latitude})',


_72


'destination':


_72


'POINT(${_selectedDestination!.longitude} ${_selectedDestination!.latitude})',


_72


'fare': _fare,


_72


}) as List<dynamic>;


_72


_72


if (response.isEmpty) {


_72


if (mounted) {


_72


ScaffoldMessenger.of(context).showSnackBar(


_72


const SnackBar(


_72


content: Text('No driver found. Please try again later.')),


_72


);


_72


}


_72


return;


_72


}


_72


String driverId = response.first\['driver_id'\];


_72


String rideId = response.first\['ride_id'\];


_72


_72


_driverSubscription = supabase


_72


.from('drivers')


_72


.stream(primaryKey: \['id'\])


_72


.eq('id', driverId)


_72


.listen((List<Map<String, dynamic>> data) {


_72


if (data.isNotEmpty) {


_72


setState(() {


_72


_driver = Driver.fromJson(data\[0\]);


_72


});


_72


_updateDriverMarker(_driver!);


_72


_adjustMapView(


_72


target: _appState == AppState.waitingForPickup


_72


? _currentLocation!


_72


: _selectedDestination!);


_72


}


_72


});


_72


_72


_rideSubscription = supabase


_72


.from('rides')


_72


.stream(primaryKey: \['id'\])


_72


.eq('id', rideId)


_72


.listen((List<Map<String, dynamic>> data) {


_72


if (data.isNotEmpty) {


_72


setState(() {


_72


final ride = Ride.fromJson(data\[0\]);


_72


if (ride.status == RideStatus.riding &&


_72


_appState != AppState.riding) {


_72


_appState = AppState.riding;


_72


} else if (ride.status == RideStatus.completed &&


_72


_appState != AppState.postRide) {


_72


_appState = AppState.postRide;


_72


_cancelSubscriptions();


_72


_showCompletionModal();


_72


}


_72


});


_72


}


_72


});


_72


_72


_goToNextState();


_72


} catch (e) {


_72


if (mounted) {


_72


ScaffoldMessenger.of(context).showSnackBar(


_72


SnackBar(content: Text('Error: ${e.toString()}')),


_72


);


_72


}


_72


}


_72


}


`


## Step 8: Updating the car icon on the map#


We will not make an app for the driver in this article, but let’s imagine we had one. As the driver’s car moves, it could update it’s position on the` drivers` table. In the previous step, we are listening to the driver’s position being updated, and using those information, we could move the car in the UI as well.


Implement` _updateDriverMarker` method, which updates the driver’s icon on the map as the position changes. We can also calculate the angle at which the driver is headed to using the previous position and the current position.


`
_44


void _updateDriverMarker(Driver driver) {


_44


setState(() {


_44


_markers.removeWhere((marker) => marker.markerId.value == 'driver');


_44


_44


double rotation = 0;


_44


if (_previousDriverLocation != null) {


_44


rotation =


_44


_calculateRotation(_previousDriverLocation!, driver.location);


_44


}


_44


_44


_markers.add(Marker(


_44


markerId: const MarkerId('driver'),


_44


position: driver.location,


_44


icon: _carIcon!,


_44


anchor: const Offset(0.5, 0.5),


_44


rotation: rotation,


_44


));


_44


_44


_previousDriverLocation = driver.location;


_44


});


_44


}


_44


_44


void _adjustMapView({required LatLng target}) {


_44


if (_driver != null && _selectedDestination != null) {


_44


LatLngBounds bounds = LatLngBounds(


_44


southwest: LatLng(


_44


min(_driver!.location.latitude, target.latitude),


_44


min(_driver!.location.longitude, target.longitude),


_44


),


_44


northeast: LatLng(


_44


max(_driver!.location.latitude, target.latitude),


_44


max(_driver!.location.longitude, target.longitude),


_44


),


_44


);


_44


_mapController?.animateCamera(CameraUpdate.newLatLngBounds(bounds, 100));


_44


}


_44


}


_44


_44


double _calculateRotation(LatLng start, LatLng end) {


_44


double latDiff = end.latitude - start.latitude;


_44


double lngDiff = end.longitude - start.longitude;


_44


double angle = atan2(lngDiff, latDiff);


_44


return angle * 180 / pi;


_44


}


`


## Step 9: Ride Completion#


Finally when the car arrives at the destination (when the driver updates the status to` completed` ), a modal thanking the user for using the app shows up. Implement` _showCompletionModal` to greet our valuable customers.


Upon closing the modal, we reset the app’s state so that the user can take another ride.


`
_36


/// Shows a modal to indicate that the ride has been completed.


_36


void _showCompletionModal() {


_36


showDialog(


_36


context: context,


_36


barrierDismissible: false,


_36


builder: (BuildContext context) {


_36


return AlertDialog(


_36


title: const Text('Ride Completed'),


_36


content: const Text(


_36


'Thank you for using our service! We hope you had a great ride.'),


_36


actions: <Widget>\[


_36


TextButton(


_36


child: const Text('Close'),


_36


onPressed: () {


_36


Navigator.of(context).pop();


_36


_resetAppState();


_36


},


_36


),


_36


\],


_36


);


_36


},


_36


);


_36


}


_36


_36


void _resetAppState() {


_36


setState(() {


_36


_appState = AppState.choosingLocation;


_36


_selectedDestination = null;


_36


_driver = null;


_36


_fare = null;


_36


_polylines.clear();


_36


_markers.clear();


_36


_previousDriverLocation = null;


_36


});


_36


_getCurrentLocation();


_36


}


`


With the edge function deployed, you should be able to run the app at this point. Note that you do need to manually tweak the driver and ride data to test out all the features. I have created a[simple script that simulates the movement and status updates of a driver](https://github.com/dshukertjr/uber-clone/tree/main/scripts/dart) so that you can enjoy the full Uber experience without actually manually updating anything from the dashboard.


You can also find the complete code[here](https://github.com/dshukertjr/uber-clone) to fully see everything put together.


## Conclusion#


This tutorial has walked you through the process of building a basic Uber clone using Flutter and Supabase. The application demonstrates how easy it is to handle real-time geospatial data using Supabase and Flutter.


This implementation serves as a foundation that can be expanded upon. Additional features such as processing payments, ride history, and driver ratings can be incorporated to enhance the application's functionality.


Want to learn more about Maps and PostGIS? Make sure to follow our[Twitter](https://x.com/supabase) and[YouTube](https://www.youtube.com/@Supabase) channels to not miss out! See you then!


## More Supabase Resources#


- [Flutter Tutorial: building a Flutter chat app](https://supabase.com/blog/flutter-tutorial-building-a-chat-app)
- [Generate Flame template using Very Good CLI](https://verygood.ventures/blog/generate-a-game-with-our-new-template)
- [Supabase Flutter SDK docs](https://supabase.com/docs/reference/dart/start)
