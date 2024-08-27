# jps-balancer-enhance
An add-on to enhance the jem balancer functionality on LB layers. It can be installed on CP2-9 or Extra layers.

It will allow you to add nodes from other layers to the load balancer configuration
```
sudo jem balancer addCommonHost -h 10.x.x.x
sudo jem balancer rebuildCommon
```

Handled events:
* onAfterRedeployContainer[nodeGroup:bl]:
  - install the scripts on the LB layer
* onBeforeRedeployContainer[${targetNodes.nodeGroup}]:
  - remove the node from the LB config if "Temporarily remove node(s) from DNS" is enabled
* onAfterRedeployContainer[${targetNodes.nodeGroup}]:
  - add the node back to the LB config if "Temporarily remove node(s) from DNS" is enabled
* onBeforeRemoveNode[${targetNodes.nodeGroup}]:
 - remove the node from the LB config before it's removed from the environment
* onAfterScaleOut:
 - add new nodes from the layer to the LB config

Menu items:
* Add nodes to LB config
 - reAdds all the layer nodes to the LB config
* Remove nodes from LB config
 - removes all the layer nodes from the LB config

Other available options:
```
Balancer module
Usage: jem balancer <action> <options>

Standard actions:
HelpDisplay help text
UsageDisplay usage information
VersionDisplay version information

Extra actions:
  AddCluster                  Add hosts to upstream group_name
  -h <host> -g <group name> 
  AddCommonHost               Add host to common group
  -h <host>                 
  AddNewHost                  Add new host to nginx configuration
  -h|--host <host>          
  AddSticky                   (no description)
  BuildCluster                Add group 'group_name' to cluster
  -g <group name>           
  Clear                       Clear upsteams hosts
  DisableProxySSL             (no description)
  EnableProxySSL              (no description)
  Help                        Display help text
  RebuildCommon               Rebuild with common config
  RebuildConfig               Rebuild nginx configuration
  RefreshWeight               Update weight value in nginx configuration
  Reload                      Load clean configs
  RemoveCommonHost            Remove host from common group
  -h <host>                 
  RemoveHost                  Remove host from nginx configuration
  -h|--host <host>          
  RemoveSticky                (no description)
  ReplaceHost                 Replace host in nginx configuration
  -s|--source <host> -d|--destination <host>
                              
  SetNeighbors                Add neighbors balancers
  SetProxyTimeout             (no description)
  SetVariables                (no description)
  UnbuildCluster              Remove group 'group_name' from cluster
  -g <group name>           
  UpdatePortMapping           (no description)
  Usage                       Display usage information
  Version                     Display version information

Actions options:
    -d|--destination            host name or IPt
    -g                          group name
    -h|--host                   host name or IP
    -h                          host name or IP
    -s|--source                 host name or IP
```