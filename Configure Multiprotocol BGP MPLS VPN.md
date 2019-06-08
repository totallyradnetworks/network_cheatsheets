# How to Configure Multiprotocol BGP MPLS VPN

Configuring Multiprotocol BGP Connectivity on the PE Devices and Route Reflectors
-

## CLI

```
enable
configure terminal
router bgp {as_number}
no bgp default ipv4 unicast(optional)
neighbor {ipaddress | peer group name} remote-as {as-number}
neighbor {ipaddress | peer group name} activate
address-family vpnv4 [unicast]
neighbor {ipaddress | peer group name} send-community extended
neighbor {ipaddress | peer group name} activate
end
```

1. router bgp 65555
   1. configures a bgp routing process and enters router configuration mode
   2. as_number
      1. identifies the device to other BGP speaking devices and tags routing information passed along
      2. range
         1. 0 65535
         2. 65412 65535 - private as_numbers / good for internal use

2. no bgp default ipv4 unicast(optional)
   1. disable the IPv4 unicast address family on all neighbors
   2. only issue this if you are using this neighbor for MPLS routes only

3. neighbor 10.0.0.1 remote-as 100
   1. adds an entry to the BGP or Multiprotocol BGP neighbor table
   2. ip_address - specifies the ip of the neighbor
   3. peer group name - specifies the name of a BGP peer group
   4. remote_as - specifies the AS to which the neighbor belongs

4. neighbor 10.0.0.1 activate
   1. enable the exchange of information with a neighboring device

5. address-family vpnv4
   1. enters address family configuration mode - routing session config for BGP and others that use VPNv4 address prefixes
   2. unicast(optional) - specifies VPNv4 unicast address prefixes

6. neighbor 10.0.0.1 send-community extended
   1. specifies that a communities attribute should be sent to a BGP neighbor
   2. ip_address - specifies the IP address the BGP speaking neighbor or bgp-peer-group-name

7. neighbor 10.0.0.1 activate
   1. enable the exchanfe of information with a neighboring device

8. end

## Configuring BGP as the Routing Protocol Between PE and CE Devices
-

```
enable
configure terminal
router bgp {as_number}
address-family ipv4 [multicast | unicast | vrf {vrf_name}]
neighbor {ip_address | peer-group name} remote-as {as_number}
neighbor {ip_address | peer-group name} activate
exit-address-family
end
```

1. router bgp 65555
   1. configures BGP process and enters router configuration mode
2. address-family ipv4 vrp vpn1
   1. specifies the ipv4 family type and enters address family configuration mode
   2. multicast - keyword specifies ipv4 multicast prefixes
   3. unicast - keyword specifies ipv4 unicast prefixes
   4. vrf - keyword and argument specifiy the name of the VRF to associate with subsequent IPv4 address family configuration mode commands
3. neighbor 10.0.0.1 remote-as 200
   1. adds an entry into the BGP table
4. neighbor 10.0.0.1 activate
   1. enables the exchange of information with a neighboring BGP device
5. exit-address-family
6. end

## VERIFY & TROUBLESHOOT COMMANDS

   1. Verifying the VPN Configuration
      1. show ip vrf
         1. This command will verify the route distinguisher(RD) and interface that are configured for the VRF
         1. a route distinguisher must be configured for the virtual routing and fowarding(VRF) instance
         2. MPLS must be configured on the interfaces that carry the VRF

   2. Verifying IP connectivity from CE Device to CE Device across the MPLS core

```
enable
ping [protocol] {hostname | system address}
trace [protocol] [destination]
show ip route [ip_address [mask] [longer prefixes]][list[access_list_name | access_list_number]]
```

   3. Verifying the local and remote CE Devices are in the PE routing table

```
enable
show ip route vrf {vrf_name} [prefix]
show ip cef vrf {vrf_name} [ip_prefix]
```
   1. show ip route vrf vpn1
      1. displays the IP routing table associated with a virtual routing and forwarding instance
      2. Check that the loopback addresses of the local and remote CE devices are in the routing table of the PE
   2. shoe ip cef vrf vpn1
      1. displays the CEF table associated with a vrf
      2. check that the prefix of the remote CE device is in the CEF table

```
show ip vrf
show ip bgp neighbor
debug ip bgp {ip_address of neighbor} event
```

