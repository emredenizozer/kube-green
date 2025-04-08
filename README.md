# kube-green

kube-green is a simple k8s addon that automatically shuts down (some of) your resources when you don't need them.

## Why?

 - With kube-green, you'll have environmentally friendly development practices through reduced energy consumption. Additionally, you'll effectively trim resource costs:

   - Are your development pods running continuously — even on weekends or overnight? This not only wastes resources but also drives up costs unnecessarily.

   - Have you ever considered the annual carbon footprint of just one pod? Based on [kube-green's estimates](https://kube-green.dev/docs/getting-started/), a single pod emits roughly 11 kg of CO₂ equivalent per year.

## How to install it?

1. Install a kubernetes cluster in your local machine:

    ```kind create cluster --name kube-green```

2. Install the cert-manager

    ```kubectl apply -f https://github.com/jetstack/cert-manager/releases/latest/download/cert-manager.yaml```

3. Install kube-green

    ```kubectl apply -f https://github.com/kube-green/kube-green/releases/latest/download/kube-green.yaml```


## How to test it?

1. **Set Up the Test Environment**

    First, create a namespace and deploy the test workloads:

    ```
    # Create a test namespace
    kubectl create ns test-sleep

    # Deploy workloads that will be affected by kube-green
    kubectl -n test-sleep create deploy nginx-1 --image=nginx --replicas=5
    kubectl -n test-sleep create deploy nginx-2 --image=nginx --replicas=2

    # Deploy a control workload (will NOT be affected by kube-green)
    kubectl -n test-sleep create deploy do-not-sleep-nginx --image=nginx --replicas=2
    ```

2. **Configure Sleep Behavior**

    Choose one of the options below for testing:

    **Option A: Time-Based Sleep (Minutes)**

    - Sleeps every 1st minute
    - Wakes up every 5th minute

        ```kubectl apply -f sleep-info-minutes.yaml```

    **Option B: Fixed Schedule Sleep**

    - Sleeps at 18:00 (6 PM)
    - Wakes up at 08:00 (8 AM) on weekdays

        ```kubectl apply -f sleep-info-fixed-interval.yaml```

### Resources

- https://kube-green.dev/ 