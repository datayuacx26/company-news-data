---
schema_version: "1.0.0"
document_id: "2aedad3eb54dce86f7b1d202e69382592eeaa3609a5a4f7a96a7be2da8136e33"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/react-native-storage"
published_at: "2023-08-01T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T21:01:50.039250+00:00"
content_hash: "sha256:699e717180fc7331cb9180dba2be84f835af7614f7e8a4d5557b3d8d212f6eca"
---

# React Native file upload with Supabase Storage

If you want to upload files from your React Native app, you need a backend to store the files,[Supabase Storage](https://supabase.com/docs/guides/storage) is a great choice for this as it provides a simple API to upload files, and we can easily combine this with authentication to build a powerful app.


This means you can quickly build your own image-sharing app, a file-sharing app, or any other app that needs to upload files to a backend!


In this tutorial, you will learn to:


- Set up a React Native app with[Expo SDK49](https://expo.dev/)
- Use[Supabase Authentication](https://supabase.com/docs/guides/auth)
- Work with Expo Router v2 and protected routes
- Upload files to[Supabase Storage](https://supabase.com/docs/guides/storage)


You can also directly check out the[full source code on GitHub](https://github.com/saimon24/react-native-resumable-upload-supabase) so you can get started with Supabase fast!


Before we get into the app, let's quickly[start a new Supabase project](https://supabase.com/dashboard) .


## Creating the Supabase Project#


To use authentication and storage we need a new Supabase project. If you don't have a Supabase account yet, you can[get started for free](https://supabase.com/dashboard) !


In your dashboard, click "New Project" and leave it to the default settings, but make sure you keep a copy of your Database password!


After a minute your project should be ready, and we can configure the authentication and storage.


## Setting up Authentication#


Authentication will be enabled by default, but we want to turn off email confirmation for this tutorial.


Select **Authentication** from the menu, go to the **Providers** section, and expand Email.


Here you can disable the confirmation email, and apply other changes later if you want to!


## Setting up Storage#


Now we want to create a **bucket** under storage where we will upload our files, and also add some security rules to protect the files of a user.


First, select **Storage** from the menu, then click **New bucket** and call it` files` .


Make sure that this is not a public bucket, otherwise, even unauthenticated users can upload or read the files!


To protect that bucket and allow users only access to their own folder, we need to add some[Storage policies](https://supabase.com/docs/guides/storage/security/access-control) .


You can either do this through the UI and pick from examples, or simply run my SQL script in the **SQL Editor** which you can select from the menu:


`
_10


CREATE POLICY "Enable storage access for users based on user_id" ON "storage"."objects"


_10


AS PERMISSIVE FOR ALL


_10


TO public


_10


USING (bucket_id = 'files' AND (SELECT auth.uid()::text )= (storage.foldername(name))\[1\])


_10


WITH CHECK (bucket_id = 'files' AND (SELECT auth.uid()::text) = (storage.foldername(name))\[1\])


`


This will allow users to only access their own folder, and not any other files in the bucket.


## Setting up the React Native app#


Now that we have our Supabase project ready, we can start building the React Native app!


Get started by setting up a new Expo app with the tabs template and install some dependencies:


`
_10


# Create a new Expo app


_10


npx create-expo-app@latest cloudApp --template tabs@49


_10


_10


# Install dependencies


_10


npm i @supabase/supabase-js


_10


npm i react-native-url-polyfill base64-arraybuffer react-native-loading-spinner-overlay @react-native-async-storage/async-storage


_10


_10


# Install Expo packages


_10


npx expo install expo-image-picker


_10


npx expo install expo-file-system


`


We will use the[Expo AsyncStorage](https://docs.expo.dev/versions/latest/sdk/async-storage/) to store the Supabase session, and the[Expo Image Picker](https://docs.expo.dev/versions/latest/sdk/imagepicker/) to select images from the device. We also need the[Expo File System](https://docs.expo.dev/versions/latest/sdk/filesystem/) to read the image from the device and upload its data.


You can now already run your project with` npx expo` and then select a platform to run on.


However, the tabs template contains a lot of code that we don't need, so to simplify things we can remove the **app** , **constants** and **components** folder.


This gives us a much cleaner project structure.


## Connecting to Supabase from React Native#


To use Supabase we need to initialize the client with our project URL and the public key, which you can find in the **Settings** of your project under **API** .


You can put both of them in a` .env` file at the root of your project:


`
_10


EXPO_PUBLIC_SUPABASE_URL=


_10


EXPO_PUBLIC_SUPABASE_ANON_KEY=


`


We can now simply read those values from the environment variables and initialize the Supabase client, so create a file at` config/initSupabase.ts` and add the following code:


`
_15


import AsyncStorage from '@react-native-async-storage/async-storage'


_15


import 'react-native-url-polyfill/auto'


_15


_15


import { createClient } from '@supabase/supabase-js'


_15


_15


const url = process.env.EXPO_PUBLIC_SUPABASE_URL


_15


const key = process.env.EXPO_PUBLIC_SUPABASE_ANON_KEY


_15


_15


// Initialize the Supabase client


_15


export const supabase = createClient(url, key, {


_15


auth: {


_15


storage: AsyncStorage,


_15


detectSessionInUrl: false,


_15


},


_15


})


`


We are using the AsyncStorage from Expo to handle the session of our Supabase client and add in the` createClient` function.


Later we can import the` supabase` client from this file and use it in our app whenever we need to access Supabase.


## Building the authentication flow#


Currently, the app won't work as we have no entry point. Because we are using the Expon Router and file-based routing, we can create a new file at` app/index.tsx` which will be the first page that comes up in our app.


On this page we will handle both login and registration, so let's start by creating a simple form with a few inputs and buttons inside the` app/index.tsx` file:


`
_98


import { Alert, View, Button, TextInput, StyleSheet, Text, TouchableOpacity } from 'react-native'


_98


import { useState } from 'react'


_98


import React from 'react'


_98


import Spinner from 'react-native-loading-spinner-overlay'


_98


import { supabase } from '../config/initSupabase'


_98


_98


const Login = () => {


_98


const \[email, setEmail\] = useState('')


_98


const \[password, setPassword\] = useState('')


_98


const \[loading, setLoading\] = useState(false)


_98


_98


// Sign in with email and password


_98


const onSignInPress = async () => {


_98


setLoading(true)


_98


_98


const { error } = await supabase.auth.signInWithPassword({


_98


email,


_98


password,


_98


})


_98


_98


if (error) Alert.alert(error.message)


_98


setLoading(false)


_98


}


_98


_98


// Create a new user


_98


const onSignUpPress = async () => {


_98


setLoading(true)


_98


const { error } = await supabase.auth.signUp({


_98


email: email,


_98


password: password,


_98


})


_98


_98


if (error) Alert.alert(error.message)


_98


setLoading(false)


_98


}


_98


_98


return (


_98


<View style={styles.container}>


_98


<Spinner visible={loading} />


_98


_98


<Text style={styles.header}>My Cloud</Text>


_98


_98


<TextInput


_98


autoCapitalize="none"


_98


placeholder="john@doe.com"


_98


value={email}


_98


onChangeText={setEmail}


_98


style={styles.inputField}


_98


/>


_98


<TextInput


_98


placeholder="password"


_98


value={password}


_98


onChangeText={setPassword}


_98


secureTextEntry


_98


style={styles.inputField}


_98


/>


_98


_98


<TouchableOpacity onPress={onSignInPress} style={styles.button}>


_98


<Text style={{ color: '#fff' }}>Sign in</Text>


_98


</TouchableOpacity>


_98


<Button onPress={onSignUpPress} title="Create Account" color={'#fff'}></Button>


_98


</View>


_98


)


_98


}


_98


_98


const styles = StyleSheet.create({


_98


container: {


_98


flex: 1,


_98


paddingTop: 200,


_98


padding: 20,


_98


backgroundColor: '#151515',


_98


},


_98


header: {


_98


fontSize: 30,


_98


textAlign: 'center',


_98


margin: 50,


_98


color: '#fff',


_98


},


_98


inputField: {


_98


marginVertical: 4,


_98


height: 50,


_98


borderWidth: 1,


_98


borderColor: '#2b825b',


_98


borderRadius: 4,


_98


padding: 10,


_98


color: '#fff',


_98


backgroundColor: '#363636',


_98


},


_98


button: {


_98


marginVertical: 15,


_98


alignItems: 'center',


_98


backgroundColor: '#2b825b',


_98


padding: 12,


_98


borderRadius: 4,


_98


},


_98


})


_98


_98


export default Login


`


There's nothing fancy going on here, but this is all we need to use Supabase Authentication in our app!


You can try it out right now and create a new user account or sign in with an existing one and log the values to the console to see what's going on.


However, we are not handling the authentication state yet so let's create a **Context** to listen to changes.


We will wrap a Provider around our app, which will use the` onAuthStateChange` function from Supabase to listen to changes in the authentication state and accordingly set our state.


For this, create a new file at` provider/AuthProvider.tsx` and add the following code:


`
_49


import React, { useState, useEffect, createContext, PropsWithChildren } from 'react'


_49


import { Session, User } from '@supabase/supabase-js'


_49


import { supabase } from '../config/initSupabase'


_49


_49


type AuthProps = {


_49


user: User | null


_49


session: Session | null


_49


initialized?: boolean


_49


signOut?: () => void


_49


}


_49


_49


export const AuthContext = createContext<Partial<AuthProps>>({})


_49


_49


// Custom hook to read the context values


_49


export function useAuth() {


_49


return React.useContext(AuthContext)


_49


}


_49


_49


export const AuthProvider = ({ children }: PropsWithChildren) => {


_49


const \[user, setUser\] = useState<User | null>()


_49


const \[session, setSession\] = useState<Session | null>(null)


_49


const \[initialized, setInitialized\] = useState<boolean>(false)


_49


_49


useEffect(() => {


_49


// Listen for changes to authentication state


_49


const { data } = supabase.auth.onAuthStateChange(async (event, session) => {


_49


setSession(session)


_49


setUser(session ? session.user : null)


_49


setInitialized(true)


_49


})


_49


return () => {


_49


data.subscription.unsubscribe()


_49


}


_49


}, \[\])


_49


_49


// Log out the user


_49


const signOut = async () => {


_49


await supabase.auth.signOut()


_49


}


_49


_49


const value = {


_49


user,


_49


session,


_49


initialized,


_49


signOut,


_49


}


_49


_49


return <AuthContext.Provider value={value}>{children}</AuthContext.Provider>


_49


}


`


To use the context we can now wrap it around our app, and while we do this we can also take care of the navigation:


In the topmost layout file we can check whether a user has an active session or not, and either directly sign the user into the inside area (that we will create soon) or automatically bring her back to the login screen.


To make this work with the Expo Router we can create a file at` app/_layout.tsx` and add the following code:


`
_38


import { Slot, useRouter, useSegments } from 'expo-router'


_38


import { useEffect } from 'react'


_38


import { AuthProvider, useAuth } from '../provider/AuthProvider'


_38


_38


// Makes sure the user is authenticated before accessing protected pages


_38


const InitialLayout = () => {


_38


const { session, initialized } = useAuth()


_38


const segments = useSegments()


_38


const router = useRouter()


_38


_38


useEffect(() => {


_38


if (!initialized) return


_38


_38


// Check if the path/url is in the (auth) group


_38


const inAuthGroup = segments\[0\] === '(auth)'


_38


_38


if (session && !inAuthGroup) {


_38


// Redirect authenticated users to the list page


_38


router.replace('/list')


_38


} else if (!session) {


_38


// Redirect unauthenticated users to the login page


_38


router.replace('/')


_38


}


_38


}, \[session, initialized\])


_38


_38


return <Slot />


_38


}


_38


_38


// Wrap the app with the AuthProvider


_38


const RootLayout = () => {


_38


return (


_38


<AuthProvider>


_38


<InitialLayout />


_38


</AuthProvider>


_38


)


_38


}


_38


_38


export default RootLayout


`


Whenever the` initialized` or` session` state changes, we check if the user is authenticated and redirect her to the correct page.


This also means we don't have to worry about the authentication state in our pages anymore, we can just assume that the user is authenticated and use the` useAuth` hook to access the user and session data later on.


Your app might show an error right now because the` /list` route doesn't exist yet, but we will create it in the next step.


## File Upload to Supabase Storage#


Now that we have the authentication set up, we can start working on the file upload.


First, let's define another layout for this inside area so create a file at` /app/(auth)/_layout.tsx` and add the following code:


`
_35


import { Stack } from 'expo-router'


_35


import { useAuth } from '../../provider/AuthProvider'


_35


import React from 'react'


_35


import { TouchableOpacity } from 'react-native'


_35


import { Ionicons } from '@expo/vector-icons'


_35


_35


// Simple stack layout within the authenticated area


_35


const StackLayout = () => {


_35


const { signOut } = useAuth()


_35


_35


return (


_35


<Stack


_35


screenOptions={{


_35


headerStyle: {


_35


backgroundColor: '#0f0f0f',


_35


},


_35


headerTintColor: '#fff',


_35


}}


_35


>


_35


<Stack.Screen


_35


name="list"


_35


options={{


_35


headerTitle: 'My Files',


_35


headerRight: () => (


_35


<TouchableOpacity onPress={signOut}>


_35


<Ionicons name="log-out-outline" size={30} color={'#fff'} />


_35


</TouchableOpacity>


_35


),


_35


}}


_35


></Stack.Screen>


_35


</Stack>


_35


)


_35


}


_35


_35


export default StackLayout


`


This defines a simple stack navigation and adds a button to trigger the logout, so we can now also fully test the authentication flow.


Next, we create the page for uploading and displaying all files of a user from Supabase Storage.


You won't have any files to show yet, but loading the files of a user is as easy as calling` list()` on the storage bucket and passing the user id as the folder name.


Additionally, we add a little FAB (floating action button) to trigger the file picker, so create a file at` /app/(auth)/list.tsx` and add the following code:


`
_63


import { View, StyleSheet, TouchableOpacity, ScrollView } from 'react-native'


_63


import React, { useEffect, useState } from 'react'


_63


import { Ionicons } from '@expo/vector-icons'


_63


import * as ImagePicker from 'expo-image-picker'


_63


import { useAuth } from '../../provider/AuthProvider'


_63


import * as FileSystem from 'expo-file-system'


_63


import { decode } from 'base64-arraybuffer'


_63


import { supabase } from '../../config/initSupabase'


_63


import { FileObject } from '@supabase/storage-js'


_63


_63


const list = () => {


_63


const { user } = useAuth()


_63


const \[files, setFiles\] = useState<FileObject\[\]>(\[\])


_63


_63


useEffect(() => {


_63


if (!user) return


_63


_63


// Load user images


_63


loadImages()


_63


}, \[user\])


_63


_63


const loadImages = async () => {


_63


const { data } = await supabase.storage.from('files').list(user!.id)


_63


if (data) {


_63


setFiles(data)


_63


}


_63


}


_63


_63


const onSelectImage = async () => {


_63


// TODO


_63


}


_63


_63


return (


_63


<View style={styles.container}>


_63


{/* FAB to add images */}


_63


<TouchableOpacity onPress={onSelectImage} style={styles.fab}>


_63


<Ionicons name="camera-outline" size={30} color={'#fff'} />


_63


</TouchableOpacity>


_63


</View>


_63


)


_63


}


_63


_63


const styles = StyleSheet.create({


_63


container: {


_63


flex: 1,


_63


padding: 20,


_63


backgroundColor: '#151515',


_63


},


_63


fab: {


_63


borderWidth: 1,


_63


alignItems: 'center',


_63


justifyContent: 'center',


_63


width: 70,


_63


position: 'absolute',


_63


bottom: 40,


_63


right: 30,


_63


height: 70,


_63


backgroundColor: '#2b825b',


_63


borderRadius: 100,


_63


},


_63


})


_63


_63


export default list


`


This should give us a nice and clean UI.


Now we can implement the image picker and upload the selected image to Supabase Storage.


Using the image picker from Expo gives us a **URI** , which we can use to read the file from the file system and convert it to a base64 string.


We can then use the` upload()` method from the storage client to upload the file to the storage bucket. Life can be easy.


At this point, you should be able to upload files to Supabase Storage, and you can already see them in your UI (or log them to the console).


To finally display them we will add a` ScrollView` component, which will render one item for every file of a user.


Let's start by creating those component rows in another file, so create a` components/ImageItem.tsx` file and add the following code:


`
_46


import { FileObject } from '@supabase/storage-js'


_46


import { Image, View, Text, TouchableOpacity } from 'react-native'


_46


import { supabase } from '../config/initSupabase'


_46


import { useState } from 'react'


_46


import { Ionicons } from '@expo/vector-icons'


_46


_46


// Image item component that displays the image from Supabase Storage and a delte button


_46


const ImageItem = ({


_46


item,


_46


userId,


_46


onRemoveImage,


_46


}: {


_46


item: FileObject


_46


userId: string


_46


onRemoveImage: () => void


_46


}) => {


_46


const \[image, setImage\] = useState<string>('')


_46


_46


supabase.storage


_46


.from('files')


_46


.download(\`${userId}/${item.name}\`)


_46


.then(({ data }) => {


_46


const fr = new FileReader()


_46


fr.readAsDataURL(data!)


_46


fr.onload = () => {


_46


setImage(fr.result as string)


_46


}


_46


})


_46


_46


return (


_46


<View style={{ flexDirection: 'row', margin: 1, alignItems: 'center', gap: 5 }}>


_46


{image ? (


_46


<Image style={{ width: 80, height: 80 }} source={{ uri: image }} />


_46


) : (


_46


<View style={{ width: 80, height: 80, backgroundColor: '#1A1A1A' }} />


_46


)}


_46


<Text style={{ flex: 1, color: '#fff' }}>{item.name}</Text>


_46


{/* Delete image button */}


_46


<TouchableOpacity onPress={onRemoveImage}>


_46


<Ionicons name="trash-outline" size={20} color={'#fff'} />


_46


</TouchableOpacity>


_46


</View>


_46


)


_46


}


_46


_46


export default ImageItem


`


This component will display the image from Supabase Storage, the name of the file and a delete button.


To display the image we use the` download()` method from the storage client, which returns a` FileObject` with the file data. We can then use the` FileReader` to convert the file data to a base64 string, which we can use as the image source.


Now let's use this component in our` list.tsx` file to render the list of images by updating the` return` statement:


`
_19


return (


_19


<View style={styles.container}>


_19


<ScrollView>


_19


{files.map((item, index) => (


_19


<ImageItem


_19


key={item.id}


_19


item={item}


_19


userId={user!.id}


_19


onRemoveImage={() => onRemoveImage(item, index)}


_19


/>


_19


))}


_19


</ScrollView>


_19


_19


{/* FAB to add images */}


_19


<TouchableOpacity onPress={onSelectImage} style={styles.fab}>


_19


<Ionicons name="camera-outline" size={30} color={'#fff'} />


_19


</TouchableOpacity>


_19


</View>


_19


)


`


Don't forget to also include the import to the` ImageItem` component!


Finally, we can also include the delete functionality by adding the following code to the` list.tsx` :


`
_10


const onRemoveImage = async (item: FileObject, listIndex: number) => {


_10


supabase.storage.from('files').remove(\[\`${user!.id}/${item.name}\`\])


_10


const newFiles = \[...files\]


_10


newFiles.splice(listIndex, 1)


_10


setFiles(newFiles)


_10


}


`


We are handling the deletion here so we can accordingly update the state of the files list after removing an item.


And with that in place, you have a fully functional image gallery app with React Native and Supabase Storage!


## What about resumable uploads?#


Initially, I wanted to include[resumable uploads](https://supabase.com/blog/storage-v3-resumable-uploads) in this tutorial, but apparently, the[Uppy](https://uppy.io/) client didn't work 100% for React Native yet.


You can still see an[initial implementation of resumable downloads with Supabase and React Native](https://github.com/saimon24/react-native-resumable-upload-supabase/commit/bb45455dc2d4d3fdd42089a8a26a521cc5b5e60f#diff-2d97fcb7ad2b1ac426a8d5391cfd0ab970e525ede1f11e14885f65e6dcd116a1) in the repository of this tutorial.


However, ultimately the uploaded file was always 0 bytes, so I decided to leave it out for now.


The Supabase team is investigating this issue, so I'm very sure that we will have resumable uploads working with React Native soon.


## Conclusion#


It's almost too easy to use Supabase Storage, and it's a great way to store files for your apps.


You now have a fully functional image gallery app with React Native and Supabase Storage including user authentication without writing a line of backend code!


You can[find the full code of this tutorial on GitHub](https://github.com/saimon24/react-native-resumable-upload-supabase) where you just need to insert your own Supabase URL and API key.


If you enjoyed the tutorial, you can[learn React Native on Galaxies.dev](https://galaxies.dev/) where I help developers build awesome React Native apps.


Until next time and happy coding with Supabase!


## More React Native/Expo resources#


- [Getting started with React Native authentication](https://supabase.com/blog/react-native-authentication)
- [Offline-first React Native Apps with Expo, WatermelonDB](https://supabase.com/blog/react-native-offline-first-watermelon-db)
- [React Native Quickstart](https://supabase.com/docs/guides/auth/quickstarts/react-native)
- [Watch our React Native video tutorials](https://youtube.com/playlist?list=PL5S4mPUpp4OsrbRTx21k34aACOgpqQGlx&si=Ez-0S4QhBxtayYsq)
