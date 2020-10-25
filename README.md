

# Notes:

```
Saving file to: /Users/colin/work/k3s/kubeconfig

# Test your cluster with:
export KUBECONFIG=/Users/colin/work/k3s/kubeconfig
kubectl get node -o wide


# Test your cluster with:
export KUBECONFIG=/Users/colin/work/geerlingguy/kubeconfig
kubectl get node -o wide

```


# Centos Vagrantfile

 

```
mkdir k3s
cd k3s
vagrant init geerlingguy/centos7
```

Set public_network in Vagrantfile
```

config.vm.network "public_network"

```
Add this to ignore shared folders plugin errors
```
config.vm.synced_folder ".", "/vagrant", disabled: true
```

You need to vagrant ssh and your copy public key to .ssh/authorized_keys manually


Set memory and cpu in Vagrantfile

```
  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
   config.vm.provider "virtualbox" do |vb|
       vb.memory = "6144"
       vb.cpus = "2"
   end
  #
```  

# Modify centos

```
   sudo yum install -y container-selinux selinux-policy-base
   sudo yum install -y https://rpm.rancher.io/k3s-selinux-0.1.1-rc1.el7.noarch.rpm
```


# Download K3SUP

```
colin@cmccarth-mac k3s % curl -sLS https://get.k3sup.dev | sh
Downloading package https://github.com/alexellis/k3sup/releases/download/0.9.6/k3sup-darwin as /Users/colin/work/k3s/k3sup-darwin
Download complete.

Running with sufficient permissions to attempt to move k3sup to /usr/local/bin
New version of k3sup installed to /usr/local/bin
 _    _____                 
| | _|___ / ___ _   _ _ __  
| |/ / |_ \/ __| | | | '_ \ 
|   < ___) \__ \ |_| | |_) |
|_|\_\____/|___/\__,_| .__/ 
                     |_|    
Version: 0.9.6
Git Commit: b94b83b4b3fda86ac88c736f0d98218bfa3cb37b

```

# Install K3SUP

No traefik

Extra features

```
export IP=192.168.0.33

k3sup install --ip $IP --user vagrant  --k3s-extra-args '--no-deploy traefik' —extra-config=apiserver.enable-admission-plugins=NamespaceLifecycle,LimitRanger,ServiceAccount,DefaultStorageClass,DefaultTolerationSeconds,NodeRestriction,MutatingAdmissionWebhook,ValidatingAdmissionWebhook,ResourceQuota,PodPreset —extra-config=apiserver.runtime-config=settings.k8s.io/v1alpha1=true
```

# Errors - fix added to steps above
```
[ERROR]  Failed to find the k3s-selinux policy, please install:
    yum install -y container-selinux selinux-policy-base
    yum install -y https://rpm.rancher.io/k3s-selinux-0.1.1-rc1.el7.noarch.rpm
```

installed these with sudo on the vagrant box and then re-ran --> k3sup install --ip $IP --user vagrant

# good to go!!


