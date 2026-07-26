# production-k8s-guestbook-stack- Architecture

                              [ User Traffic ]
                                       │
                                       ▼
                       [ Ingress Controller (Nginx) ]
                                (TLS / HTTPS)
                                       │
                                       ▼
                         [ Service: frontend (ClusterIP) ]
                                       │
             ┌─────────────────────────┼─────────────────────────┐
             ▼                         ▼                         ▼
      [ Pod: frontend-1 ]       [ Pod: frontend-2 ]       [ Pod: frontend-3 ]
      (Autoscaled via HPA)      (Autoscaled via HPA)      (Autoscaled via HPA)
             │                         │                         │
      (Writes)                         └────────────┐     (Reads)│
             │                                      ▼            │
             ▼                            [ Service: redis-follower ]
[ Service: redis-master ]                           │
             │                         ┌────────────┴────────────┐
             ▼                         ▼                         ▼
   [ StatefulSet: master ]     [ Pod: follower-1 ]       [ Pod: frontend-2 ]
             │                         │                         │
             ▼                         ▼                         ▼
     [ PVC: 10Gi Disk ]        [ PVC: 10Gi Disk ]        [ PVC: 10Gi Disk ]
