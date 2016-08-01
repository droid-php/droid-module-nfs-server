# Droid Module: droid/nfs-server

Install and configure an NFS server and shares. For more information on Droid,
please see [droidphp.com](http://droidphp.com).

The steps involved are:-

1. Update platform packages.
2. Install the nfs-server package.
3. Create directories to be shared.
4. Write configuration to /etc/exports.
5. Stop and then start the NFS Kernel service.


## Assumptions

1. The platform is Debian-based.
2. An NFS server is not already installed and exporting shares - any existing
   configuration is overwritten.


## Limitations

1. Requires portmapper and rpcbind for client discoverability.
2. No security hardening is performed.


## Information required by the module

1. A list of exports, as follows:-

        module_nfs_server:
          exports:
            -
              path: <string> # The path to be exported
              common_opts: <string> # Export options common to all clients
              clients: # List of clients
                -
                  name: <string> # hostname or IP address of a permitted client
                  opts: <string> # Optional client-specific export options

2. A list of directories to be shared (exported):-

        module_nfs_server_shares:
          -
            path: <string> # path to a directory to create and export
            mode: <string || number> # file mode of the directory


## Optional information

None.
