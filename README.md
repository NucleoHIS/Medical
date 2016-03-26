Medical
========
O projeto usa  HTML5, CSS e JavaScript para o front-end e usa  REST webservices provided do OpenMRS.
Usamos  Sencha Touch 2 e Sencha ExtJS 4.1 o  Framework para MVC e UI, e usamos também Jasmine 1.1.0 é usado para Testes Baseados em Comportamento.

A árvore do projeto está configurado da seguinte forma:

     /src    
        /app                 -Login Module (Sencha Touch 2.0) for Raxa-JSS EMR
        /chw                 -Community Health Worker Module
        /data                -Stores the data shared with all other modules
        /laboratory          -Laboratory module (using Sencha ExtJS 4.1)
        /lib                 -Contains Library files (Sencha ExtJS & Sencha Touch)
        /outpatient          -Outpatient module (using Sencha Touch 2.0)         
        /patient-facing      -Files for the Patient Module will be stored here
        /pharmacy            -Pharmacy module (using Sencha Touch 4.1)           
        /registration        -Registration module (using Sencha Touch 2.0)
        /registrationextjs4  -Registration module (using Sencha ExtJS 4.1)       
        /resources           -common resources shared with other modules    
           /common           -common xtypes needed on all screens eg. topbar
           /css              -stores cascading style sheet
           /img              -stores images needed in all modules
           /script           -stores scripts needed in various modules
        /screener            -Screener module (using Sencha Touch 2.0)
        /voice               -Voice Module files to be added here
     /test                   -Test Module (using Jasmine 1.1.0)
        /lib                 -Contains testing library (Jasmine 1.1.0)
        /specs               -Test specs of every module goes into corresponding folder
              

Sencha Workspace
=============

[Workspace documentation] (http://docs.sencha.com/ext-js/4-1/#!/guide/command_workspace)

**Adding New Apps**

If you want to make a new app, cd to the ```lib/ext``` folder and run ```sencha generate app <appNamespace> <pathToAppFolder>```. To generate a new sencha app, cd to ```lib/touch``` and run the same command. This will appropriately generate the app with the correct path to the library files, sencha/workspace configs for building, etc.

**Building Apps**
When building apps, pay special attention to ```.sencha/workspace/sencha.cfg``` and ```src/<module>/.sencha/app/sencha.cfg```. The config file specifies folders that are included during the Ant build process.

**Webapp**

To create 'compiled' webapp builds, run the following sencha command:
```
sencha app build <build_type>
```
where ```<build_type>``` can be ```testing```, ```production```, or ```package```.

For more information on the role of each of these build types, please see the sencha cmd documentation. (Note that the ```production``` webapp build is not yet working for Outpatient. This version concatenates most of the applications JavaScript files, but for any .js files referred to in app.json, it fetches those JavaScript files dynamically. This causes the app to fail to load, if you include a file like Util.js in app.json. The reason is that Util.js contains which has global variables and functions which are called during setup, but because the files haven't been fetched yet, setup fails.)

**Native**

To create the native (device or emulator) builds, Sencha touch uses the packager.json file to configure build options.

- [iOs Packaging](http://docs.sencha.com/touch/2-0/#!/guide/native_packaging)
- [Android Packaging](http://docs.sencha.com/touch/2-0/#!/guide/native_android)
- [iOs Provisioning](http://docs.sencha.com/touch/2-0/#!/guide/native_provisioning)

To build, exceute the following command 
```
sencha app build native
```

**Common files**

Common files - e.g. models, stores, views - should go in ```/src/common/```. Group them like plugins, e.g.

```
/src/common/<plugin_name>
/src/common/<plugin_name>/model/plugin.js
/src/common/<plugin_name>/store/plugin.js
/src/common/<plugin_name>/view/mainView.js
/src/common/<plugin_name>/view/modalDialog.js
```

This way it's easy to include a plugin in an application by 

First, adding a SetPath command to ```app.js```

```
Ext.Loader.setPath({
    'RaxaEmr.common.<plugin_name>': '/src/common/<plugin_name≥', 
    ...
});
```

Next, updating ```src/<module>/.sencha/app/sencha.cfg``` to include the plugin
```
app.<plugin_name> = ${app.dir}/../common/<plugin_name>
app.classpath=${app.dir}/app.js,${app.dir}/app,${app.<plugin_name>}

```
