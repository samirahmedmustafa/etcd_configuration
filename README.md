# etcd_configuration

- Generate the CA using the below command (pay attention to CA:TRUE)

  openssl genrsa -out ca.key 4096

  openssl req -x509 -new -nodes \
    -key ca.key \
    -sha256 \
    -days 3650 \
    -subj "/CN=etcd-root-ca" \
    -addext "basicConstraints=critical,CA:TRUE" \
    -addext "keyUsage=critical,keyCertSign,cRLSign" \
    -addext "subjectKeyIdentifier=hash" \
    -addext "authorityKeyIdentifier=keyid:always,issuer" \
    -out ca.crt

- Create and edit the servers openssl configuration files such as mentioned [server1](etcd-server1.cnf) and [server2](etcd-server2.cnf)
- Generate the server certificates and sign them as below

  openssl genrsa -out etcd-node1.key 2048
  openssl req -new -key etcd-server1.key -out etcd-server1.csr -config etcd-server1.cnf
  openssl x509 -req -in etcd-server1.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out etcd-server1.crt -days 1000 -extensions v3_req -extfile etcd-server1.cnf

  openssl genrsa -out etcd-server2.key 2048
  openssl req -new -key etcd-server2.key -out etcd-server2.csr -config etcd-server2.cnf
  openssl x509 -req -in etcd-server2.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out etcd-server2.crt -days 1000 -extensions v3_req -extfile etcd-server2.cnf

- Download etcd, extract it and move it to a standard binaries location

  wget https://github.com/etcd-io/etcd/releases/download/v3.6.7/etcd-v3.6.7-linux-amd64.tar.gz
  tar -xzf etcd-v3.6.7-linux-amd64.tar.gz
  cp etcd-v3.6.7-linux-amd64/etcd* /usr/bin/

- Create the systemd service files for the 2 servers as mentioned in the links [server1](SERVER1etcd.service) and [server2](SERVER2etcd.service) those files should be renamed to etcd.service in the 2 servers

- Start etcd services in the 2 servers

	systemctl daemon-reload

	systemctl start etcd
