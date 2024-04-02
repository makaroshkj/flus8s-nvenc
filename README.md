This example is just a demostration how to run Flussonic nvenc transcoder in [k3s](https://docs.k3s.io/quick-start).

**Prerequisites**
* Docker installed and k3s installed
```
curl -sfL https://get.k3s.io | sh -
mkdir /root/.kube
cp /etc/rancher/k3s/k3s.yaml /root/.kube/config
```
* Nvidia driver installed. The test was for GeForce RTX 2070 with `535.161.07` driver and `12.2` CUDA version.

**How to run it**
* [k8s-device-plugin](https://github.com/NVIDIA/k8s-device-plugin) should be used but however a few additional options are required to run it in k3s. Enable modified k8s-device-plugin:
```
kubectl apply -f nvidia-device-plugin-custom.yml
```
* Add your `LICENSE_KEY` in `flussonic-pod.yml` and apply it:
```
kubectl apply -f flussonic-pod.yml
```
* Pod and transcoding should be running:
```
kubectl get all -n flussonic
NAME            READY   STATUS    RESTARTS   AGE
pod/nvenc-pod   1/1     Running   0          35s
```
```
kubectl logs pod/nvenc-pod -n flussonic
{"time":"2024-04-02T18:42:49.959165Z","event":"transcoder_started","media":"fake","module":"live_stream_input","line":985,"transcoder_session_id":"660c51a9-7917-4dfd-8a3e-f8ec59958dea","hw":"nvenc"}
``