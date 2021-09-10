# Monitoring and Auto-scaling

![](./img/Cloud_watch_diagram.PNG)

## Auto-scaling setup
### Creating a launch template

We need to create a launch template taht specifies the type of EC2 we want tolaunch and the AMI that we want to use.
1. Select `Launch Templates` in the side bar and then select `Create launchtemplate`.
2. Enter a logical name for the template.
3. Choose the relevant AMI for template. *(SRE_will_app_AMI)*
4. Choose `t2.micro` as the `Instance type`.
5. Choose the correct key pair to use. *(sre_key)*
6. In `Network settings`, leave `VPC` as the chosen option.
7. Choose the relevant security group.
8. Leave the storage volume as the default.
9. Leave `Network interfaces` and `Advanced details` alone. These are not required.
10. Select `Create launch template`
11. On the confirmation page, select `Create Auto Scaling group`

### Create a load balancer

We need a load balancer to direct traffic to our EC2 instances.
1. Navigate to `Load Balancer` on the side bar and select `Create Load Balancer`
2. Select the `Application Load Balancer`
3. Give the load balancer a name.
4. Choose `Internet-facing` and `IPv4`
5. Under `Network mapping`, leave the default VPC selected and select all 3subnets.
6. Under `Security groups`, you can add more than one security group for the loadbalancer. Leave only the default security group selected.
7. Under `Listeners and routing`, select a target group (or choose `Create targetgroup`)

    ### Creating a target group
    1. Choose a target type > Instances
    2. Give the target group a name
    3. Leave the selected `Protocol` as HTTP and `Port` as 80
    4. Leave all other settings as they are and choose `Next`
    5. On the `Register targets` page, select your running instances that shouldhave been started when you created your Auto Scaling group
    6. Select `Create target group`

8. Select `Create load balancer`

### Creating an Auto Scaling group

The Auto Scaling group will 
1. Enter a logical name for your Auto Scaling group. *(SRE_will_AS_group)*
2. Select the minimum and maximum capacity of the Auto Scaling group. *(Desiredcapacity is the number of instances that the group will launch upon its creation)*
3. Select the launch template you want to use. *This box will automatically selectthe launch template you just created.*
4. Choose `Next`. This takes you to the networking configuration page.
5. Leave the `Adhere to launch template` option selected.
6. Under `Network`, choose the VPC you want to use as well as the subnet. *In thiscase, we are going to use the default VPC and one of the defualt subnets.*
7. Under `Load balancing`, select the first option and choose your target group.
8. Select `Create Auto Scaling group`.

### Verify the Auto Scaling group

Now that the Auto Scaling group has been created, we want to verify that it haslaunched an EC2 instance.
1. Navigate to `Auto Scaling Groups` on the sidebar. Select your group.
2. In the detials box that appears on the bottom half of the window, select the`Activity` tab.
3. Go to `Activity history` and you should see that an EC2 instance wassuccessfully launched and the time at which it was launched.
4. In the `Instance management` tab, we can look at the status of all the activeinstances.

### Setup automatic Auto Scaling

We want to monitor the active instances and perform actions (like creating newinstances) when we think it is required.
1. Navigate to `Auto Scaling Groups` on the side bar, and find your group.
2. Go to the `Automatic scaling` tab and select `Create dynamic scaling policy`
3. - Policy type > Target tracking scaling
   - Scaling policy name > *Give it a logical name*
   - Metric type > Average CPU utilization
   - Target value > 50
   - Instances need \**leave blank*\* seconds warm up before including metric
4. Select `Create`

The auto scaling policy has now been created and will spin up new EC2 instances ifthe total CPU utilization goes above 50%.