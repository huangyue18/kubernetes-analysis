## build Kubernetes images \(v1.7.11\)

1. git clone kubernetes
2. git checkout v1.7.11
3. pull base images
   gcr.io/google\_containers/kube-cross:v1.8.3-1 \(kubernetes/build/build-image/cross/VERSION\)  
   gcr.io/google-containers/debian-iptables-amd64:v7 \(kubernetes/build/common.sh\)  
   busybox

4. edit kubernetes/build/lib/release.sh
   line 333:

   ```bash
   "${DOCKER[@]}" build --pull -q -t "${docker_image_tag}" ${docker_build_path} >/dev/null
   ```

   remove "--pull -q"

5. make quick-release



