# Cloud Infrastructure:
### How would you recover access to an instance that you have lost all ssh keys for?

1.  First thing we can used we can try to ssh on instance from browser.But in most of the orgnisations this ssh option and cloudshell is blocked.

2.  Detach and Attach the Root Volume :
    1.  Stop the instance.
    2.  Detach the root EBS volume.
    3.  Attach the root EBS volume to another instance as a secondary volume.
    4.  SSH into the second instance.
    5.  Modify the SSH authorized_keys file on the attached volume to include a new key.
    6.  Detach the volume from the second instance.
    7.  Reattach it to the original instance as the root volume.
    8.  Start the original instance and SSH into it using the new key.


### GCP (Google Cloud Platform)
In gcp you can enabled the os-login field in your instance and then you can simply used gcloud CLI in order to login to vm.

```
gcloud compute instances add-metadata INSTANCE_NAME \
    --metadata enable-oslogin=TRUE

```

Provide access roles = roles/compute.osLogin and roles/compute.osAdminLogin.

then 
authenticate
add your key

```
gcloud auth login

gcloud compute os-login ssh-keys add --key-file ~/.ssh/my-oslogin-key.pub

ssh <user name>@<IP>

```
by this approch no need to manage keys it's take care by google
at any point of time you can add new ssh key


### How would you ensure that a preemptible/spot instance always came up with the same external IP address?

Steps:
1.  Reserve a Static External IP Address
2.  Create a Preemptible Instance with the Static IP
3.  Create an Instance Template with the Static IP inorder to ensuring the Static IP is Reused.
4.  Create a Managed Instance Group.
5.  Set Autohealing Policies and will also get autohealing(Optional)

### How would you ensure that a given instance could write to a specific GCS/S3 bucket without giving the instance any API keys?


### GCP
1.  Create a Service Account.
2.  Give necessary permissions to SA. (roles/storage.objectCreator)
3.  While creating VM attach SA to it when VM is created gcloud setup with this SA is already there.
4. You can access Bucket objects using gsutil commands.

### AWS
1.  Create an IAM Role
2.  Attach a Policy to the Role.(you can create custom write policy as well s3-write-policy.json)
3.  Launch the EC2 Instance with the IAM Role
4.  You can access Bucket objects using AWS CLI commands.