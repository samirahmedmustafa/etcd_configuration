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

- Generate the servers openssl configuration files such as mentioned [server1](etcd-server1.cnf) [server2](etcd-server2.cnf)
