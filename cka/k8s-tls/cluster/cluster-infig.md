# cluster level info
1. Node dev-worker label vm=prod
2. Node dev-worker2 label vm=dev
3. Node dev-worker taint hw=hp:NoSchedule
4. Node dev-worker2 taint hw=lp:PrefereNoSchedule
# fetch pods from different namespace
1. bash simple script
   - for ns in dev default; do kubectl get pods -n $ns;done
2. kubectl getpods -n ns -w watch pods as old pods are deleted and new pods are created 