# QGIS

[Home](../../readme.md) > [SIG](../sig.md) > [QGIS](./qgis.md)

> **QGIS** is a professional GIS application that is built on top of and proud to be itself Free and Open Source Software (FOSS) ([qgis.org](https://www.qgis.org/en/site/))

## Content

- [QGIS in Docker](./docker.md)

## Documentation

- Main: [en](https://www.qgis.org/en/docs/index.html), [fr](https://www.qgis.org/fr/docs/index.html)
- [Demo](http://demo.qgis.org/)
- [The graphical modeler](https://docs.qgis.org/2.8/en/docs/user_manual/processing/modeler.html)

## Plugins

Visit [QGIS plugins web portal](https://plugins.qgis.org/).

| Name | Details |
| ---- | ------- |
| OGR2Layers | [github](https://github.com/lucadelu/OGR2Layers/) |
| QuickMapServices | [github](http://nextgis.com/blog/quickmapservices/) |
| Qgis2threejs | [qgis2threejs.readthedocs.io](http://qgis2threejs.readthedocs.io/en/docs-release/), [examples](http://qgis2threejs.readthedocs.io/en/docs-release/Examples.html) |
| mmqgis | [plugins.qgis.org](https://plugins.qgis.org/plugins/mmqgis/), [github](https://github.com/michaelminn/mmqgis) |
| Plugin reloader | [github](https://github.com/borysiasty/plugin_reloader) |
| Plugin builder| [github](https://github.com/g-sherman/Qgis-Plugin-Builder) |

### How-to create a QGIS plugin

- Reference

  - [QGIS Coding Standards](https://www.qgis.org/en/site/getinvolved/development/qgisdevelopersguide/codingstandards.html)
  - [Developing Python Plugins](https://docs.qgis.org/testing/en/docs/pyqgis_developer_cookbook/plugins.html)
  - [Building a Python Plugin](http://www.qgistutorials.com/en/docs/building_a_python_plugin.html)

- Steps

  - Download [osgeo4w](http://trac.osgeo.org/osgeo4w/) (doesn't work... seems not needed), [QtCreator](https://www.qt.io/) ([offline-installer](https://www1.qt.io/offline-installers/))
  - Install QGIS plugins (extensions): Plugin Builder, Plugin Reloader (_experimental_)
  - Look at `C:\Users\username\.qgis2\python\plugins`, this is where plugins are loaded
  - Edit the file `C:\Program Files\QGIS 2.18\bin\o4w_env.bat` and add `C:\Qt\Qt5.10.1\Tools\mingw530_32\bin` to SET PATH commmand line, then open a dos window.
  - Install MinGW

  ```dos
  cd C:\Qt\Qt5.10.1\Tools\mingw530_32\bin
  copy mingw32-make.exe make.exe
  ```

  - In Windows, search for "OSGeo4W Shell", open the application.

  ```dos
  cd C:\Users\bertrand.thomas\.qgis2\python\plugins\MyPluginName
  make
  REM pyrcc4 -o resources.py  resources.qrc
  ```

  - Think about reload the plugin repository when the plugin directory is directly updated!
