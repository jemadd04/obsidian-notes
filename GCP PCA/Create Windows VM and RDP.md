
After creating the VM....

Run *gcloud compute instances get-serial-port-output instance_name*. If prompted, type N and press Enter.
	This tells you whether the server instance is ready for an RDP connection.
	Should return *Instance setup finished. instance_name is ready to use.*

## RDP into the Windows Server

1. Set a password: *gcloud compute reset-windows-password [instance] --zone [zone] --user [username]*
2. Enter Y when prompted.
3. Connect via RDP with [admin] as the username and the password previously set