# Rolling Updates and Rollbacks in Kubernetes

Rolling updates and rollbacks are essential features in Kubernetes that allow for seamless deployment and management of applications. In this documentation, we will explore how to perform rolling updates and rollbacks in Kubernetes.

## Rolling Updates

Rolling updates refer to the process of updating an application by gradually replacing instances of the old version with instances of the new version. This approach ensures that the application remains available during the update process, minimizing downtime.

To perform a rolling update in Kubernetes, you can use the `kubectl set image` command. This command allows you to update the image of a specific container within a deployment or a statefulset. By specifying the new image, Kubernetes will automatically initiate the rolling update process.

During a rolling update, Kubernetes follows a predefined strategy to ensure a smooth transition. It gradually terminates the old instances and creates new instances with the updated image. This process continues until all instances have been updated.

## Rollbacks

Rollbacks are necessary when an update introduces issues or errors that affect the application's stability or functionality. Kubernetes provides a straightforward mechanism to rollback to a previous version of an application.

To perform a rollback in Kubernetes, you can use the `kubectl rollout undo` command. This command allows you to revert to the previous version of a deployment or a statefulset. Kubernetes will automatically handle the rollback process by terminating the new instances and recreating the old instances.

During a rollback, Kubernetes ensures that the application remains available by maintaining a minimum number of replicas. This ensures that the rollback process does not cause any downtime for the application.

## Commands

Here are some common commands used for performing rolling updates and rollbacks in Kubernetes:

### Rolling Updates

- Update the image of a container in a deployment:

  ```bash
  kubectl set image deployment/<deployment-name> <container-name>=<new-image>
  ```

- Update the image of a container in a statefulset:
  ```bash
    kubectl set image statefulset/<statefulset-name> <container-name>=<new-image>
  ```

### Rollbacks

- Rollback to the previous version of a deployment:

  ```bash
  kubectl rollout undo deployment/<deployment-name>
  ```

- Rollback to the previous version of a statefulset:

  ```bash
    kubectl rollout undo statefulset/<statefulset-name>
  ```

- Rollback to a specific revision of a deployment:

  ```bash
    kubectl rollout undo deployment/<deployment-name> --to-revision=<revision-number>
  ```

- Rollback status

  ```bash
     kubectl rollout status deployment/<deployment-name>
  ```

- Check the history of a deployment

  ```bash
     kubectl rollout history deployment/<deployment-name>
  ```

  - Check the history of a deployment with revision number

  ```bash
     kubectl rollout history deployment/<deployment-name> --revision=<revision-number>
  ```

By using these commands, you can easily perform rolling updates and rollbacks in Kubernetes, ensuring a smooth deployment process and maintaining the availability of your applications.
