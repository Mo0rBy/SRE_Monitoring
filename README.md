# Monitoring and Auto-scaling

![](./img/Cloud_watch_diagram.PNG)

## Auto-scaling setup
1. Creating a launch template

    We need to create a launch template taht specifies the type of EC2 we want to launch and the AMI that we want to use.
    1. Select `Launch Templates` in the side bar and then select `Create launch template`.
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

2. Creating an Auto Scaling group

    The Auto Scaling group will 
    1. Enter a logical name for your Auto Scaling group. *(SRE_will_AS_group)*
    2. Select the minimum and maximum capacity of the Auto Scaling group. *(Desired capacity is the number of instances that the group will launch upon its creation)*
    3. Select the launch template you want to use. *This box will automatically select the launch template you just created.*
    4. Choose `Next`. This takes you to the networking configuration page.
    5. Leave the `Adhere to launch template` option selected.
    6. Under `Network`, choose the VPC you want to use as well as the subnet. *In this case, we are going to use the default VPC and one of the defualt subnets.*
    7. For now, **skip to review**
    8. Select `Create Auto Scaling group`.

3. Verify the Auto Scaling group

    Now that the Auto Scaling group has been created, we want to verify that it has launched an EC2 instance.
    1. Navigate to `Auto Scaling Groups` on the sidebar. Select your group.
    2. In the detials box that appears on the bottom half of the window, select the `Activity` tab.
    3. Go to `Activity history` and you should see that an EC2 instance was successfully launched and the time at which it was launched.
    4. In the `Instance management` tab, we can look at the status of all the active instances. 
