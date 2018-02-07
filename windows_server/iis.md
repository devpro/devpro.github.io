# IIS

## inetsrv

You may experience some issues while using appcmd.exe if the IIS configuration is invalid.
In this case, look at the error details and double check the `applicationHost.config` file.

Possible locations:

- `%windir%\System32\inetsrv\Config\applicationHost.config`
- `%windir%\SysWOW64\inetsrv\Config\applicationHost.config`
- `%SystemRoot%\Sysnative\inetsrv\config\applicationHost.config`
