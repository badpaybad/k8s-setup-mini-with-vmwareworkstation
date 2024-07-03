# k8s-setup-mini-with-vmwareworkstation

https://kubernetes.io/docs/tasks/tools/

https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/


# all vm 
 
                <!-- curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

                sudo chmod 777 kubectl


                sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl -->

                sudo apt-get update
                # apt-transport-https may be a dummy package; if so, you can skip that package
                sudo apt-get install -y apt-transport-https ca-certificates curl gpg


                sudo swapoff -a
                sudo sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab

                sudo modprobe overlay
                sudo modprobe br_netfilter

                sudo tee /etc/modules-load.d/k8s.conf <<EOF
                overlay
                br_netfilter
                EOF

                sudo tee /etc/sysctl.d/k8s.conf <<EOF
                net.bridge.bridge-nf-call-ip6tables = 1
                net.bridge.bridge-nf-call-iptables = 1
                net.ipv4.ip_forward = 1
                EOF

                sudo sysctl --system


                sudo apt-get install -y docker.io
                sudo systemctl enable docker
                sudo systemctl start docker


                # If the directory `/etc/apt/keyrings` does not exist, it should be created before the curl command, read the note below.
                # sudo mkdir -p -m 755 /etc/apt/keyrings
                curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.30/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg


                echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.30/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list

                # sudo nano /etc/apt/sources.list.d/kubernetes.list

                sudo apt update
                
                sudo apt-get install -y kubelet kubeadm kubectl


# control plane

                sudo kubeadm init --apiserver-advertise-address=0.0.0.0 --pod-network-cidr=192.168.0.0/16 
                #--cri-socket unix:///run/containerd/containerd.sock

                mkdir -p $HOME/.kube
                sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
                sudo chown $(id -u):$(id -g) $HOME/.kube/config

                export KUBECONFIG=/etc/kubernetes/admin.conf

                sudo kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml



                kubeadm token create --print-join-command

                got bellow , should run in Worker node 

                kubeadm join <control-plane-ip>:6443 --token <token> --discovery-token-ca-cert-hash sha256:<hash> --cri-socket unix:///run/containerd/containerd.sock


# allow all port 

sudo ip6tables -F
sudo ip6tables -X
sudo ip6tables -Z
sudo ip6tables -P INPUT ACCEPT
ksudo ip6tables -P FORWARD ACCEPT
sudo ip6tables -P OUTPUT ACCEPT


sudo netfilter-persistent save


