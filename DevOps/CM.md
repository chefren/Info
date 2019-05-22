# Configuration Management
Problems to solve:
- Mass deployment: can't do manually
- Move from testing to production: Need equal environments
- App failures: need quick rollback mechanisms

## IaC
Infrastructure as Code was created with similar characteristics
- Provision nodes (Dev, Test, Prod)
- Automated updates
  - Push
  - Pull (from nodes)
- Move from shell scripting into CM scripts to reduce code and have tool libraries

## Popular tools (as of April 2019)
This is subjectively weighted, trying to be fair. As it turns out each tool always has pros and cons, so you need to see what works best in your work/environment.

||[Puppet](https://puppet.com/)|[Saltstack](https://www.saltstack.com/)|[Ansible](https://www.ansible.com/)|[Chef](https://www.chef.io/get-chef/)
--- | :---: | :---: | :---: | :---:
Scale|High|H|H|
Setup ease|Master/Agent|M/A|Master/Node (Best)|M/A
Availability|Multi-Master|M-M|Primary/Secondary|P/S
MGM|Pull - Own DSL|Push|Push|Pull - Own Ruby DSL
Interop|Master Agent Unix/Win|"|"|"
Lang|DSL|YAML (python)|YAML (python)|Ruby/DSL
[GH](https://github.com) Activity|Stable - High release|H - LR|H - LR|S - HR
Cost (100 nodes)|12k|15k|10k|7k
Popularity|High-Stable|Up|H|H-S
Success case|NYSE|Linkedin|Ratmap|Gannett