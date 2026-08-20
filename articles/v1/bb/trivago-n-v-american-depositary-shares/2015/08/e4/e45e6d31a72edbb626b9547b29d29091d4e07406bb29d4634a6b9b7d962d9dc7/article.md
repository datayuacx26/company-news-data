---
schema_version: "1.0.0"
document_id: "e45e6d31a72edbb626b9547b29d29091d4e07406bb29d4634a6b9b7d962d9dc7"
company_key: "trivago-n-v-american-depositary-shares"
company: "trivago N.V."
source_id: "trivago-n-v-american-depositary-shares-rss-0be0766927d8"
canonical_url: "https://tech.trivago.com/post/2015-08-26-writing-to-legacy-session/"
published_at: "2015-08-26T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:44.896445+00:00"
fetched_at: "2026-07-28T22:27:37.879348+00:00"
content_hash: "sha256:61d93852f804ab80cbc64e91c5a297577540ed7306eadb2a10757c67c37ef687"
---

# Keeping in sync: Sharing sessions between Symfony applications

Learn how we managed to move fast and create a new Symfony application without breaking our old legacy session handling. We write to our legacy session (which is file based) from our new project which uses[PDO](http://symfony.com/doc/current/cookbook/configuration/pdo_session_storage.html) as the session storage.


We all have been at a point where we have to support a legacy application while working on new projects. In our case we had a two year old application using[Symfony 2.0](http://www.symfony.com/) that we wanted to keep stable. We did not want to make any big changes to it anymore.


We already started a new project based on Symfony 2.5. At the beginning we used the default session storage of Symfony called[PHP native session storage](http://php.net/manual/en/intro.session.php) . It stores the session data as text files.


There were a couple of issues with this approach:


1. We wanted one central login system for both applications
2. The legacy application couldn’t be changed, which means we should do the central login handling via the new application
3. The whole session structure was different
4. The token that we used in legacy was not reusable in the new application. (Symfony stores the whole token object in the session)


This is how the old session data looked like


```text
'_symfony2'  :  >
array   (  size  =  3  )
'attributes'  :  >
array   (  size  =  1  )
'_security_secured_area'  :  >   string   'C:74:"Symfony\Component\Security\Core\Authentication\Token\UsernamePasswordToken":255:{a:3:{i:0;N;i:1;s:12:"secured_area";i:2;s:206:"a:4:{i:0;O:29:OurOldUserClass"
'  flashes  ':>
array (size=0)
empty
'  locale  ':> string '  en  ' (length=2)
```


And the new session


```text
array   (  size  =  3  )
'_sf2_attributes'  :  >   &
array   (  size  =  2  )
'_csrf/authenticate'  :  >   string   '********'   (  length  =  43  )
'_security_main'  :  >   string   'C:74:"Symfony\Component\Security\Core\Authentication\Token\UsernamePasswordToken":601:{a:3:{i:0;N;i:1;s:4:"main";i:2;s:561:"a:4:{i:0;C:27:"OurNewClass"
'  _sf2_flashes  ':> &
array (size=0)
empty
'  _sf2_meta  ':> &
array (size=3)
'  u  ':> int 1433859366
'  c  ':> int 1433858758
'  l  ':> string '  0  ' (length=1)
```


As you can see, the whole structure has changed! Basically Symfony stopped using only one index to keep all the values it needs and moved to use a prefix for different indexes.


The initial approach was to avoid using the Session object of Symfony in the new project and just use` $_SESSION` in the success handler, which worked pretty fine.


The main issues became apparent when we started to use the PDO session handler.


Symfony uses the` session_set_save_handler` function with a handler you choose. So basically after our change, any reading or writing of` $_SESSION` was only affecting PDO and not the file based session.


That’s when we thought: “Why not do it in PHP?”
We could write the session ourselves to the corresponding file.


Unfortunately there’s no clear documentation for writing to the session file manually in PHP. So we had to some research on our own.


### The main things we learnt during our investigation


1. PHP provides a function which serializes the current` $_SESSION` variable ([session_encode](http://php.net/manual/en/function.session-encode.php) )
2. [session_save_path](http://php.net/manual/en/function.session-save-path.php) actually doesn’t return the used path for the session files. When it’s not defined it returns an empty string! In this case the session files are stored in[sys_get_temp_dir()](http://php.net/manual/en/function.sys-get-temp-dir.php)
3. The output of[sys_get_temp_dir()](http://php.net/manual/en/function.sys-get-temp-dir.php) depends on the Operating System. In some cases it returns a trailing slash and in some not. That is why we applied` rtrim` on the return value.
4. The session prefix (` _sess` ) is actually[hard coded](http://stackoverflow.com/questions/4009245/session-file-naming) in the C implementation of PHP.


Once we had gathered all the information we needed, we managed to write the session data directly to the file. Here you have the source code of our legacy session class.


```text
use   Symfony\Component\Security\Core\Authentication\Token\UsernamePasswordToken  ;


class   LegacySession
{
const   SESSION_INDEX  :   '_symfony2'  ;


public   function   getRedirectUrl  ($defaultUrl)
{
if   (  isset  ($_SESSION[  self::  SESSION_INDEX  ][  'attributes'  ][  '_security.target_path'  ]))
{
return   $_SESSION[  self::  SESSION_INDEX  ][  'attributes'  ][  '_security.target_path'  ];
}


return   $defaultUrl;
}


public   function   setToken  (  UsernamePasswordToken   $oToken)
{
$_SESSION[  self::  SESSION_INDEX  ][  'attributes'  ][  '_security_secured_area'  ]:   serialize  ($oToken);
$this  ->  prepareSession  ();
$this  ->  updateSession  ();
}


protected   function   prepareSession  ()
{
$_SESSION[  self::  SESSION_INDEX  ][  'flashes'  ]:   array  ();
$_SESSION[  self::  SESSION_INDEX  ][  'locale'  ]:   'en'  ;
}


public   function   isLoggedIn  ()
{
if   (  isset  ($_SESSION[  self::  SESSION_INDEX  ][  'attributes'  ][  '_security_secured_area'  ])) {
$_oToken:   unserialize  ($_SESSION[  self::  SESSION_INDEX  ][  'attributes'  ][  '_security_secured_area'  ]);


return   $_oToken   instanceof   UsernamePasswordToken   &&   $_oToken  ->  getUser  ()  ->  getUsername  ();
}


return   false  ;
}


protected   function   updateSession  ()
{
file_put_contents  (  $this  ->  getSessionFile  (),   session_encode  ());
}


protected   function   getSessionFile  ()
{
$_sSessionDirectory:   session_save_path  ();
if   (  !  $_sSessionDirectory)
{
$_sSessionDirectory:   rtrim  (  sys_get_temp_dir  (),   '/'  );
}


return   $_sSessionDirectory   .   '/sess_'   .   session_id  ();
}
}
```


### Things to consider


1. Using this approach is not a good idea for anything other than login and logout since you might end up with concurrency problems.
2. Even though it has minimal impact, you’re still doing a write operation on the hard disk.
3. There’s no guarantee that this approach works for every version of PHP (so far tested on PHP 5.4 and 5.5)
