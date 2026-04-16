## Post-deploy steps that may need to be addressed

# After boot, disable cloud-init network mgmt:
`sudo tee /etc/cloud/cloud.cfg.d/99-disable-network-config.cfg > /dev/null <<EOF
network: {config: disabled}
EOF`

`ls -l /etc/netplan/`

`sudo rm -f /etc/netplan/*.yaml`

# Create a clean netplan file (correct permissions)
`sudo nano /etc/netplan/01-netcfg.yaml`

`network:
  version: 2
  renderer: networkd
  ethernets:
    enp0s6:
      dhcp4: true
      dhcp4-overrides:
        use-dns: false
      nameservers:
        addresses:
          - 8.8.8.8
          - 1.1.1.1`

# Fix permissions
`sudo chmod 600 /etc/netplan/01-netcfg.yaml`

`sudo chown root:root /etc/netplan/01-netcfg.yaml`

# Apply net plan cleanly
`sudo netplan generate`

`sudo netplan apply`

# Verify resolver state
`resolvectl status enp0s6`

# Authoritative dns test 
Check for the correct url, this too can be a problem!

`getent hosts get.coolify.io`

# After dns resolves
`curl -fsSL https://get.coolify.io | bash`

`curl -fsSL https://cdn.coollabs.io/coolify/install.sh | sudo bash`

`docker ps`

# Restart example
`http://<control-node-public-ip>:8000`

# Other
# Force the trusted domain directly into the file
`docker exec -it owncloud sed -i "s/'trusted_domains' => array (/'trusted_domains' => array ( 0 => 'documents.mydomain.com',/g" /mnt/data/config/config.php`

# Force the overwrite protocol to HTTPS
`docker exec -it owncloud sed -i "/'dbpassword' =>/a \  'overwrite.cli.url' => 'https://documents.mydomain.com',\n  'overwriteprotocol' => 'https',\n  'overwritehost' => 'documents.mydomain.com'," /mnt/data/config/config.php`

# Force the CLI URL to your domain
`docker exec -it owncloud sed -i "s|'overwrite.cli.url' => 'https://localhost'|'overwrite.cli.url' => 'https://documents.mydomain.com'|g" /mnt/data/config/config.php`

# Force the protocol to HTTPS 
(this fixes the duplicate if the lines are identical)

`docker exec -it owncloud sed -i "s|'overwriteprotocol' => 'http'|'overwriteprotocol' => 'https'|g" /mnt/data/config/config.php`

# Remove the redundant localhost line 
(specifically if it still exists)

`docker exec -it owncloud sed -i "/localhost/d" /mnt/data/config/config.php`

`docker exec -it owncloud cat /mnt/data/config/config.php`

`docker restart owncloud`

# To configure external storage (sftp) back to homelab storage
Install and configure tailscale on the PC/Server and on the worker node. Once these are both signed in, they will be able to communicate with each other.
