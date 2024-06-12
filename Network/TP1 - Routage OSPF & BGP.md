# Configuration des routeurs sur AS100

## Adressage IP des routeurs AS100

R11 :
```plaintext
fa 0/0 : 192.168.11.1 255.255.255.0
gi 0/0/0 : 192.168.15.2 255.255.255.0
gi 0/1/0 : 192.168.41.1 255.255.255.0
gi 0/2/0 : 192.168.14.2 255.255.255.0
```

R12 :
```plaintext
fa 0/0 : 192.168.12.1 255.255.255.0
gi 0/0/0 : 192.168.16.1 255.255.255.0
gi 0/1/0 : 192.168.14.1 255.255.255.0
```

R13 :
```plaintext
fa 0/0 : 192.168.13.1 255.255.255.0
gi 0/0/0 : 192.168.15.1 255.255.255.0
gi 0/1/0 : 192.168.16.2 255.255.255.0
gi 0/2/0 : 192.168.42.2 255.255.255.0
```

## Routage dynamique avec OSPF

R11 : 
```plaintext
router ospf 1
network 192.168.11.0 0.0.0.255 area 100
network 192.168.14.0 0.0.0.255 area 100
network 192.168.15.0 0.0.0.255 area 100
passive-interface fa 0/0 (vu que c'est une interface qui ne renvoie vers aucun routeur)
```

R12 :
```plaintext
router ospf 1
network 192.168.12.0 0.0.0.255 area 100
network 192.168.14.0 0.0.0.255 area 100
network 192.168.16.0 0.0.0.255 area 100
passive-interface fa 0/0 (vu que c'est une interface qui ne renvoie vers aucun routeur)
```

R13 : 
```plaintext
router ospf 1
network 192.168.13.0 0.0.0.255 area 100
network 192.168.15.0 0.0.0.255 area 100
network 192.168.16.0 0.0.0.255 area 100
passive-interface fa 0/0 (vu que c'est une interface qui ne renvoie vers aucun routeur)
```

# Configuration des routeurs sur AS200

## Adressage IP des routeurs AS200

R21 :
```plaintext
gi 0/0/0 : 192.168.24.1 255.255.255.0
gi 0/1/0 : 192.168.41.2 255.255.255.0
gi 0/2/0 : 192.168.21.2 255.255.255.0
```

R22 : 
```plaintext
fa 0/0 : 192.168.22.1 255.255.255.0 
gi 0/0/0 : 192.168.21.1 255.255.255.0
gi 0/2/0 : 192.168.43.2 255.255.255.0
```

R23 :
```plaintext
fa 0/0 : 192.168.23.1 255.255.255.0 
gi 0/0/0 : 192.168.24.2 255.255.255.0
```

## Routage dynamique avec OSPF

R21 :
```plaintext
router ospf 1
network 192.168.21.0 0.0.0.255 area 200
network 192.168.24.0 0.0.0.255 area 200
```

R22 :
```plaintext 
router ospf 1
network 192.168.21.0 0.0.0.255 area 200
network 192.168.22.0 0.0.0.255 area 200
passive-interface fa 0/0 (vu que c'est une interface qui ne renvoie vers aucun routeur)
```

R23 : 
```plaintext
router ospf 1
network 192.168.23.0 0.0.0.255 area 200
network 192.168.24.0 0.0.0.255 area 200
passive-interface fa 0/0 (vu que c'est une interface qui ne renvoie vers aucun routeur)
```

# Configuration des routeurs sur AS300 :

## Adressage IP des routeurs AS300

R31 : 
```plaintext
fa 0/0 192.168.31.1 255.255.255.0
gi 0/0/0 : 192.168.35.1 255.255.255.0
gi 0/1/0 : 192.168.36.2 255.255.255.0
```

R32 :
```plaintext
fa 0/0 : 192.168.32.1 255.255.255.0 
gi 0/0/0 : 192.168.38.1 255.255.255.0 
gi 0/1/0 : 192.168.42.1 255.255.255.0 
gi 0/2/0 : 192.1368.43.1 255.255.255.0 
gi 0/3/0 : 192.168.35.2 255.255.255.0 
```

R33 :
```plaintext
fa 0/0 : 192.168.33.1 255.255.255.0 
gi 0/0/0 : 192.168.37.2 255.255.255.0 
gi 0/1/0 : 192.168.36.1 255.255.255.0 
```

R34 : 
```plaintext
fa 0/0 : 192.168.34.1 255.255.255.0 
gi 0/0/0 : 192.168.38.2 255.255.255.0 
gi 0/1/0 : 192.168.37.1 255.255.255.
```

## Routage dynamique avec OSPF

R31 : 
```plaintext
router ospf 1 
network 192.168.31.0 0.0.0.255 area 300
network 192.168.35.0 0.0.0.255 area 300
network 192.168.36.0 0.0.0.255 area 300
passive-interface fa 0/0
```

R32 : 
```plaintext
router ospf 1
network 192.168.32.0 0.0.0.255 area 300
network 192.168.35.0 0.0.0.255 area 300
network 192.168.38.0 0.0.0.255 area 300
passive-interface fa 0/0
```

R33 : 
```plaintext
router ospf 1 
network 192.168.33.0 0.0.0.255 area 300
network 192.168.36.0 0.0.0.255 area 300
network 192.168.37.0 0.0.0.255 area 300
passive-interface fa 0/0
```

R34 : 
```plaintext
router ospf 1
network 192.168.34.0 0.0.0.255 area 300
network 192.168.37.0 0.0.0.255 area 300
network 192.168.38.0 0.0.0.255 area 300
passive-interface fa 0/0
```

# Configuration BGP sur AS100 : 

R11 : 
```plaintext
router bgp 100
neighbor 192.168.41.2 remote-as 200 
network 192.168.11.0 
network 192.168.14.0
network 192.168.15.0
```

R13 : 
```plaintext 
router bgp 100
network 192.168.13.0
network 192.168.15.0
network 192.168.16.0
neighbor 192.168.42.2 remote-as 300
```

# Configuration BGP sur AS200 : 

R21 : 
```plaintext
router bgp 200
neighbor 192.168.41.1 remote-as 100
network 192.168.21.0
network 192.168.24.0
```

R22 : 
```plaintext
router bgp 200
network 192.168.21.0
network 192.168.22.0
neighbor 192.168.43.1 remote-as 300
```

# Configuration BGP sur AS300 : 

R32 :
```plaintext
router bgp 300
network 192.168.32.0
network 192.168.35.0
network 192.168.38.0
neighbor 192.168.42.1 remote-as 100
neighbor 192.168.43.2 remote-as 200
```

# Redistribution des routes entre OSPF et BGP sur AS100

R11 : 
```plaintext
router ospf 1
redistribute bgp 100 subnets
exit
router bgp 100
redistribute ospf 1
```

R13 :
```plaintext
router ospf 1
redistribute bgp 100 subnets
exit
router bgp 100
redistribute ospf 1
```

# Redistribution des routes entre OSPF et BGP sur AS200

R21 :
```plaintext
router ospf 1
redistribute bgp 200 subnets
exit
router bgp 200
redistribute ospf 1
```

R22 :
```plaintext
router ospf 1
redistribute bgp 200 subnets
exit
router bgp 200
redistribute ospf 1
```

# Redistribution des routes entre OSPF et BGP sur AS300

R32 : 
```plaintext
router ospf 1
redistribute bgp 300 subnets
exit
router bgp 300
redistribute ospf 1
```
