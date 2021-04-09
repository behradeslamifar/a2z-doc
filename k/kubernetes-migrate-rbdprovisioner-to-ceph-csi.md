# Migrate from rbd-provisioner to ceph-csi
This is two complecated process and we decided to didnt migration task online. But i hope this document be useful.
* [Blog post usr as a refrence](https://leoh0.github.io/post/2018-09-09-how-to-migrate-ceph-rbd-to-csi-ceph-rbd-in-k8s/)

## Script
This is only documented.
```
#!/usr/bin/env bash

source values

dependency-check() {
	if [ "$(which uuid)" == "" ]
	then
		echo "\"uuid\" not installed."
		exit 1
	fi
}

## Migrate details
# 1. Choose pv
# 2. Change "persistentVolumeReclaimPolicy" from "Delete" to "Retain"
#      kubectl get pv ${PV} -o yaml | \     sed 's/persistentVolumeReclaimPolicy: Delete/persistentVolumeReclaimPolicy: Retain/g' | \     kubectl replace -f -
# 3. Save PV and PVC name 
#      read NAMESPACE PVCNAME <<< \   $(kubectl get pv $PV \     -o go-template='{{ .spec.claimRef.namespace }}  {{ .spec.claimRef.name }}')
# 4. Save Pod name
#      PODNAME=$(kubectl get pods -n $NAMESPACE -o go-template="""  {{- range .items -}}  {{\$metadata:=.metadata}}  {{- range .spec.volumes -}}  {{- if .persistentVolumeClaim -}}  {{- if eq .persistentVolumeClaim.claimName \"$PVCNAME\" -}}  {{- printf \"%s\\n\" \$metadata.name -}}  {{- end }}  {{- end }}  {{- end }}  {{- end }}""")
# 5. Convert PV and PVC yaml file to csi version
# 6. Delete PVC
# 7. Delete Pod and PV (ToDO)
# 8. Create new PV and PVC
# 9. Create Pod

csi-pv() {
	## Changes
	# 1. "annotations.pv.kubernetes.io/provisioned-by: ceph.com/rbd" to "annotations.pv.kubernetes.io/provisioned-by: rbd.csi.ceph.com"
	# 2. Drop "annotations.rbdProvisionerIdentity: ceph.com/rbd"
	# 3. Drop "claimRef"
	# 4. Convert "rbd" to "csi".
	#    We need save these data:
	#    a. name and namespace
	#    b. Image name
	#    c. Delete claimRef
	#    d. 
        #      rbd:
        #        fsType: ext4
        #        image: kubernetes-dynamic-pvc-1b02d901-6ab7-11eb-8496-fe4fa0b86a0c
        #        keyring: /etc/ceph/keyring
        #        monitors:
        #        - 172.19.3.101:6789
        #        - 172.19.3.102:6789
        #        - 172.19.3.103:6789
        #        pool: RBD
        #        secretRef:
        #          name: hicap-test
        #          namespace: kube-system
        #        user: kuber.rbd.hicap
       
        #      csi:
        #        controllerPublishSecretRef:
        #          name: csi-rbd-secret
        #          namespace: ceph-csi
        #        driver: rbd.csi.ceph.com
        #        fsType: ext4
        #        nodePublishSecretRef:
        #          name: csi-rbd-secret
        #          namespace: ceph-csi
        #        nodeStageSecretRef:
        #          name: csi-rbd-secret
        #          namespace: ceph-csi
        #        volumeAttributes:
        #          clusterID: 0603ffb5-a6da-4dbc-ab4d-aab667d2bc86
        #          imageFeatures: layering
        #          imageName: kubernetes-dynamic-pvc-cff78a42-7f20-11eb-8086-96683acce539
        #          journalPool: RBD
        #          pool: RBD
        #          radosNamespace: ""
        #          storage.kubernetes.io/csiProvisionerIdentity: 1615106424998-8081-rbd.csi.ceph.com
        #          volumeNamePrefix: kubernetes-dynamic-pvc-
}

csi-pvc() {
	echo NaN
}
```
