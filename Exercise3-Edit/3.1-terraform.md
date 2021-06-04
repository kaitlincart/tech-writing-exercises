# Exercise 3.1

Edit the following tutorial to be consistent with both the Engineering style guide and Learn style guide. 

## Tutorial 

So far you’ve deployed one VCN, two Subnets, an Object Storage bucket and a compute instance.  You didn’t specify a private IP address to use for the compute instance, so your instance just got handed a random IP address.  Wouldn’t it be nice if you could see the IP address of the compute instance you deployed, printed to the display, when you run Terraform?  This is really easy to do, with the use of outputs (output variables).

## Output Configuration

Create a new file, called outputs.tf.

```shell-session
$ echo > outputs.tf
```

Now modify the contents of outputs.tf to be the following.

```diff hideClipboard
+ output "vcn_state" {
+   description = "The state of the VCN."
+   value       = oci_core_vcn.test.state
+ }
```

~> NOTE: Outputs can be defined in any Terraform code file (*.tf), however it’s a nice idea to get into the practice of separating your code into logical files so the code base is easier to navigate.  This is why you chose to place the output definition in outputs.tf, which seems like a logical place to put outputs.

## Viewing the Data

Running terraform plan won't show you the outputs - this happens when terraform apply is run.  Run terraform apply to see what it shows.

```shell-session hideClipboard
$ terraform apply
oci_core_vcn.test: Refreshing state... [id=ocid1.vcn.oc1.phx.12345ABCDEF]
oci_core_subnet.dev: Refreshing state... [id=ocid1.subnet.oc1.phx.12345ABCDEF]
oci_core_subnet.test: Refreshing state... [id=ocid1.subnet.oc1.phx.12345ABCDEF]

An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:

Terraform will perform the following actions:

Plan: 0 to add, 0 to change, 0 to destroy.

Changes to Outputs:
  + vcn_state = "AVAILABLE"

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes


Apply complete! Resources: 0 added, 0 changed, 0 destroyed.

Outputs:

vcn_state = "AVAILABLE"
$
```

Your VCN should have a state of AVAILABLE (which is what you expect at this point, as it was built quite several lessons back).

Isn’t that great!  The value of an output can be any programmatic calculation supported in Terraform code, allowing you to display a lot of useful data.  See this at work with a super simple example.  Add the following to your outputs.tf.

```diff hideClipboard
  output "instance_ip" {
    description = "The private IP of the compute instance."
    value       = oci_core_instance.app.private_ip
  }

+ output "two_plus_two" {
+   value       = 2+2
+ }
```

Now re-run terraform apply.

```shell-session hideClipboard
$ terraform apply
oci_core_vcn.test: Refreshing state... [id=ocid1.vcn.oc1.phx.12345ABCDEF]
oci_core_subnet.test: Refreshing state... [id=ocid1.subnet.oc1.phx.12345ABCDEF]
oci_core_subnet.dev: Refreshing state... [id=ocid1.subnet.oc1.phx.12345ABCDEF]

An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:

Terraform will perform the following actions:

Plan: 0 to add, 0 to change, 0 to destroy.

Changes to Outputs:
  + two_plus_two = 4

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes


Apply complete! Resources: 0 added, 0 changed, 0 destroyed.

Outputs:

two_plus_two = 4
vcn_state = "AVAILABLE"
$
```

This is valuable when using Terraform in automated pipelines as well as when running it manually (so users can see useful data).

You've covered a lot of ground, learning and applying many different Terraform features.  Before your journey ends, the [next tutorial](/tutorials/terraform/oci-remote) looks at some options around storing your Terraform state.  On with the adventure!