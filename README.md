# BPC-BPL 2. cvičenie

### Obsah cvičenia
- práca s mavros
- simulácia Gazebo, Rviz
- tvorba vlastnej témy
- tvorba vlastného servisu


Ako prvé budeme potrebovať spustiť Ardupilot SITL (Software in the loop)
- prostredie máme už pripravané


```
cd ardu_ws
```

nakoľko sa nejedná o knižnicu, ktorá je vstavaná v základnej inštalácií ROS-u, je potrebné sourcnuť .bash z knižnice
```
source install/setup.bash
```

následne môžeme spustiť simulátor
```
ros2 launch ardupilot_gz_bringup iris_runway.launch.py
```

po spustení simulátora, môžeme spustiť mavros
```
ros2 launch mavros apm.launch fcu_url:=udp://:14550@localhost:14555 gcs_url:="udp://" tgt_system:=1 tgt_component:=1
```

nasledne môžete zistiť typ správy pomocou ktorej sa zdiela stav:
- batérie
- GPS
- stav UAV (state)
- IMU
