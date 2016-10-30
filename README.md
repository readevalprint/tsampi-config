
	$ openssl genrsa 1024 > tsampi.key
	$ openssl req -new -x509 -nodes -sha1 -days 365 -key tsampi.key -out tsampi.crt

	$ kubectl create secret generic ssl --from-file=crt=./tsampi.crt --from-file=key=./tsampi.key

	$ mkdir gpg-keys   # generate your gpg private keys here
	$ kubectl create secret generic gpg-keys --from-file=./gpg-keys/

	$ kubectl create secret generic dot-ssh --from-file=id-rsa=~/.ssh/id_rsa  --from-file=id-rsa-pub=~/.ssh/id_rsa.pub  --from-file=known-hosts=~/.ssh/known_hosts
	$ kubectl create secret generic db --from-literal=password=my_secret_password

	$ kubectl apply -f deployment.yaml

	$ minikube service tsampi-service

should open browser to tsampi and now we need to set up the db

	$ kubectl get pods
	NAME                                 READY     STATUS      RESTARTS   AGE
	db-3321131789-liapl                  1/1       Running     0          46s
	tsampi-deployment-3739799081-10ysx   1/2       Completed   2          46s

	$ kubectl exec tsampi-deployment-3739799081-10ysx env COMUNS=$COLUMNS LINES=$LINES -it /code/entrypoint.sh setupdb

	$ kubectl exec tsampi-deployment-3739799081-10ysx env COMUNS=$COLUMNS LINES=$LINES  -it /code/entrypoint.sh manage createsuperuser


now try to log in the admin.

	$ minikube service tsampi-service --url

goto that url with `/admin` appended to it.
