# how to make key, certificate, keystore

openssl genpkey -algorithm RSA -aes256 > private_key.pem
openssl rsa -in private_key.pem -pubout -outform PEM -out public_key.pem
openssl req -x509 -key private_key.pem -subj /CN=showka.com -days 99999  > certificate.pem
openssl pkcs12 -export -inkey private_key.pem -in certificate.pem -out oxalis.p12 -name "peppolap"

# how to build

mvn clean install -Pdist -Dmaven.test.skip=true

# how to execute sending

cd /oxalis-dist/oxalis-standalone

java -jar target/oxalis-standalone.jar -f ~/.oxalis/files/peppol-bis-invoice-sbdh.xml -u http://localhost:8080/oxalis/as2 -cert ~/.oxalis/key/certificate.pem
