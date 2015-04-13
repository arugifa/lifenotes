=============================
Permission denied (publickey)
=============================

Last day, after upgraded my system, I encountered a weird error when trying to connect with SSH to a remote server::

    [alexandre@laptop]$ ssh remote-server.lan
    Permission denied (publickey).

So, what's going on? Since I haven't changed my SSH configuration. By trying to connect in verbose mode, I had the following output::

    [alexandre@laptop ~]$ ssh -v remote-server.lan
    OpenSSH_6.8p1, OpenSSL 1.0.2a 19 Mar 2015
    debug1: Reading configuration data /etc/ssh/ssh_config
    debug1: Connecting to remote-server.lan [1.2.3.4] port 22.
    debug1: Connection established.
    debug1: identity file /home/alexandre/.ssh/id_rsa type 1
    debug1: key_load_public: No such file or directory
    debug1: identity file /home/alexandre/.ssh/id_rsa-cert type -1
    debug1: key_load_public: No such file or directory
    debug1: identity file /home/alexandre/.ssh/id_dsa type -1
    debug1: key_load_public: No such file or directory
    debug1: identity file /home/alexandre/.ssh/id_dsa-cert type -1
    debug1: key_load_public: No such file or directory
    debug1: identity file /home/alexandre/.ssh/id_ecdsa type -1
    debug1: key_load_public: No such file or directory
    debug1: identity file /home/alexandre/.ssh/id_ecdsa-cert type -1
    debug1: key_load_public: No such file or directory
    debug1: identity file /home/alexandre/.ssh/id_ed25519 type -1
    debug1: key_load_public: No such file or directory
    debug1: identity file /home/alexandre/.ssh/id_ed25519-cert type -1
    debug1: Enabling compatibility mode for protocol 2.0
    debug1: Local version string SSH-2.0-OpenSSH_6.8
    debug1: Remote protocol version 2.0, remote software version OpenSSH_6.7p1 Debian-3
    debug1: match: OpenSSH_6.7p1 Debian-3 pat OpenSSH* compat 0x04000000
    debug1: SSH2_MSG_KEXINIT sent
    debug1: SSH2_MSG_KEXINIT received
    debug1: kex: server->client aes128-ctr umac-64-etm@openssh.com none
    debug1: kex: client->server aes128-ctr umac-64-etm@openssh.com none
    debug1: expecting SSH2_MSG_KEX_ECDH_REPLY
    debug1: Server host key: ecdsa-sha2-nistp256 SHA256:puRyQ3ifMNcLDk20Dew6hlj9t8nTWeYRWTEpjelRGCG
    debug1: Host 'remote-server.lan' is known and matches the ECDSA host key.
    debug1: Found key in /home/alexandre/.ssh/known_hosts:3
    debug1: SSH2_MSG_NEWKEYS sent
    debug1: expecting SSH2_MSG_NEWKEYS
    debug1: SSH2_MSG_NEWKEYS received
    debug1: Roaming not allowed by server
    debug1: SSH2_MSG_SERVICE_REQUEST sent
    debug1: SSH2_MSG_SERVICE_ACCEPT received
    debug1: Authentications that can continue: publickey
    debug1: Next authentication method: publickey
    debug1: Offering RSA public key: /home/alexandre/.ssh/id_rsa
    debug1: Authentications that can continue: publickey
    debug1: Trying private key: /home/alexandre/.ssh/id_dsa
    debug1: Trying private key: /home/alexandre/.ssh/id_ecdsa
    debug1: Trying private key: /home/alexandre/.ssh/id_ed25519
    debug1: No more authentication methods to try.
    Permission denied (publickey).

It seams that my SSH client tried to only use a set of private keys: id_rsa, id_dsa, id_ecdsa, id_ed25519. Of course, the key I usually use to connect on my remote server is not here!

After some searches, I discovered that my system uses ``ssh-agent`` to manage my private keys. Hence, I succeed to solve my problem by adding the key I need to the authentication agent::

    [alexandre@laptop]$ ssh-add ~/.ssh/remote-server.lan_rsa
    Enter passphrase for /home/alexandre/.ssh/remote-server.lan_rsa: 
    Identity added: /home/alexandre/.ssh/remote-server.lan_rsa (/home/alexandre/.ssh/remote-server.lan_rsa)


Resources
---------

    - http://www.kelvinwong.ca/2011/03/30/multiple-ssh-private-keys-identityfile/
    - http://mah.everybody.org/docs/ssh
