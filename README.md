Role Name
=========

Setup ECS cluster with it's instances.

Requirements
------------

For instance, if the role uses the EC2 module, Ansible2.3+, boto3, and AWS credentials.

Role Variables
--------------

```yaml
ecs_clusters:                                     # the list of ECS cluster configurations to launch
  - name: "ecs-sample"                            # the name of ECS cluster
    region: "us-east-1"                           # the AWS region to launch ECS cluster
    key_name: "my-ssh-key"                        # SSH key to add in instances
    vpc_id: "vpc-abcd1234"                        # VPC to launch the cluster
    security_groups:                              # the list of security groups to attach to the cluster instances
      - my-sg-ecs-sample-private
    ami_id: "ami-abcd1234"                        # AMI ID to setup the Launch Configuration Group
    instance_type: "t2.medium"                    # Instance type to launch the cluster
    asg_min_size: 1                               # minimal instances to setup LCG
    asg_max_size: 2                               # max of instances to setup LCG
    instance_profile_name: "ecs-role-sample"      # IAM role name to attach to the instance
    zones:                                        # List of AZs to launch instances
      - us-east-1a
    subnets:                                      # List of subnets
      - subnet-abcd1234
    cluster_type: ec2                             # Cluster type: ec2 (defaut value) or fargate

ecr_repositories:
  - name: "chaordic/platform"
    region: "us-east-1"

# To create a fargate cluster you must define the properties below:
ecs_clusters:
  - name: "ecs-fargate"
    cluster_type: fargate
```

```yml
ecs_taskdefinitions:
  - family: nginx
    containers:
    - name: nginx
      essential: true
      image: "nginx"
      portMappings:
      - containerPort: 8080
        hostPort: 8080
      cpu: 512
      memory: 1GB
    state: present
```
For all task definitions properties please look at the module [documentation](https://docs.ansible.com/ansible/latest/modules/ecs_taskdefinition_module.html)


Dependencies
------------

* Ansible 2.3+
* Boto3

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - name: Setup ECS cluster
      hosts: localhost
      connection: local
      gather_facts: yes

      vars:
        ecs_clusters:
          - name: "ecs-sample"
            region: "us-east-1"
            key_name: "my-ssh-key"
            vpc_id: "vpc-abcd1234"
            security_groups:
              - my-sg-ecs-sample-private
            ami_id: "ami-abcd1234"
            instance_type: "t2.medium"
            asg_min_size: 1
            asg_max_size: 2
            instance_profile_name: "ecs-role-sample"
            zones:
              - us-east-1a
            subnets:
              - subnet-abcd1234
            cluster_type: ec2

      roles:
        - role: cloud-ecs

License
-------

GPLv3

Author Information
------------------

Cloud Infrastructure Team, Linx Impulse
