# Droid Module: droid/nfs-server

Install and configure an NFS server and shares. For more information on Droid,
please see [droidphp.com](http://droidphp.com).

The steps involved are:-

1. Update platform packages.
2. Install the nfs-server package.
3. Create directories to be shared.
4. Write configuration files.
5. Stop and then start the NFS Kernel and NFS Common services.


## Assumptions

1. The platform is Debian-based.
2. An NFS server is not already installed and exporting shares - any existing
   configuration is overwritten.


## Limitations

1. Requires portmapper and rpcbind for client discoverability.
2. Any host configured to statically assign a port number to the lockd daemon
   (see Optional Information, below) will require a reboot.


## Configuration files managed by the module

Configuration files managed by the module will be overwritten each time the
module is run.

1. The following existing configuration files are overwritten:-
   - /etc/hosts.allow
   - /etc/hosts.deny
2. The following configuration files are installed by their respective platform
   packages during the execution of the module and then overwritten:-
   - /etc/exports
   - /etc/default/nfs-common
   - /etc/default/nfs-kernel-server
3. The following new configuration files are written:-
   - /etc/modprobe.d/lockd.conf


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

1. Per-host entries in hosts.allow and hosts.deny to secure access to NFS
   related daemons by way of tcpwrappers:-

        hosts:
          nfs_server:
            variables:
              hosts_deny:
                - <string> # suggest: "lockd: ALL"
                           #          "mountd: ALL"
                           #          "rpcbind: ALL"
                           #          "rquotad: ALL"
                           #          "statd: ALL"
              hosts_allow:
                - <string> # suggest: "lockd: 10.0.1.80 10.0.1.81"
                           #          "mountd: 10.0.1.80 10.0.1.81"
                           #          "rpcbind: 10.0.1.80 10.0.1.81"
                           #          "rquotad: 10.0.1.80 10.0.1.81"
                           #          "statd: 10.0.1.80 10.0.1.81"

2. Per-host default options in /etc/default/nfs-common and nfs-kernel-server to
   statically assign port numbers to the statd and mountd daemons:-

        hosts:
          nfs_server:
            variables:
              statd_opts:
                port: <integer>
                outgoing_port: <integer>
              mountd_opts:
                port: <integer>

3. Per-host kernel options to statically assign TCP and UDP ports to the lockd
   daemon:-

        hosts:
          nfs_server:
            variables:
              lockd_opts:
                port: <integer>
