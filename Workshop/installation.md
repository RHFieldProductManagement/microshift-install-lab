# Installing MicroShift from an RPM Package

As our system is already pre-registered to use the necessary repositories, we can go forward and install Microshift along with its dependancies :

    $ sudo dnf -y install microshift openshift-clients

Once it's done installing, we need to copy our pull-secret to the */etc/crio* folder of our RHEL VM and set the correct permissions :

    $ sudo cp $HOME/openshift-pull-secret /etc/crio/openshift-pull-secret
    $ sudo chown root:root /etc/crio/openshift-pull-secret
    $ sudo chmod 600 /etc/crio/openshift-pull-secret

## Configure the LVM Storage for MicroShift

Since we didn't use the default "rhel" name for the VG we created, we have to create a volume definition for MicroShift to use our newly created Volume Group :

    $ cat <<EOF > $HOME/lvmd.yaml 
    socket-name:
    device-classes:
      - name: vmdisk
        volume-group: vmdisk
        spare-gb: 10
        default: true
    EOF
    $ sudo cp lvmd.yaml /etc/microshift/
    
## Start MicroShift

We can finally enable microshift on the VM :

    $ sudo systemctl enable microshift --now

We will have to grab the config file and give it the correct permissions :

    $ mkdir ~/.kube
    $ sudo cp /var/lib/microshift/resources/kubeadmin/kubeconfig ~/.kube/config
    $ sudo chown -R lab-user:lab-user ~/.kube/config

We can now connect to our MicroShift cluster : 

    $ oc get pods -A

Which should give us a list ofMicroShift pods running on the system

<pre>
NAMESPACE                  NAME                                  READY   STATUS    RESTARTS   AGE
openshift-dns              dns-default-qdknf                     2/2     Running   0          4m39s
openshift-dns              node-resolver-l87bm                   1/1     Running   0          5m40s
openshift-ingress          router-default-84dcd7bcf6-np7dl       1/1     Running   0          5m34s
openshift-ovn-kubernetes   ovnkube-master-n2xtb                  4/4     Running   0          5m40s
openshift-ovn-kubernetes   ovnkube-node-pxlb7                    1/1     Running   0          5m40s
openshift-service-ca       service-ca-54c746cbb7-j5j4n           1/1     Running   0          5m34s
openshift-storage          topolvm-controller-55b6f5f56b-jszmb   4/4     Running   0          5m40s
openshift-storage          topolvm-node-6sdm6                    4/4     Running   0          4m39s
</pre>

This is the end of this lab. Please feel free to play a bit with the deployed cluster :)