## WSL

# list distributions
wsl -l

# powershell download ubuntu 16.04
Invoke-WebRequest -Uri https://aka.ms/wsl-ubuntu-1604 -OutFile Ubuntu.appx -UseBasicParsing 

# powerhell deploy package
Add-AppxPackage .\Ubuntu.appx

