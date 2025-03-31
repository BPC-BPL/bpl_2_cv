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

------
Z tretej časti si nakopirujte bpl_interfaces

v zložke srv si môžeme pozrieť náš custom service
pre tvorbu custom správ potrebujeme v zlozke msg vytvoriť formát našich správ

subor v zložke môžeme vytvoriť napr. pomocou nano
```
nano nazov suboru :)
```
Battery.msg
```
float32 voltage_v
float32 current
float32 remaining_percent
```
Gpsposition.msg
```
float64 latitude_deg
float64 longitude_deg
float32 absolute_altitude_m
float32 relative_altitude_m
int32 number_of_satellites
int32 fix_type
```
Rcstatus.msg
```
bool was_available_once
bool is_available
float32 signal_strength_percent
```

následne je ešte potrebné pridať v Cmakeliste cestu k jednotlivým témam a servisom
```
"msg/Battery.msg"
"msg/Gpsposition.msg"
"msg/Rcstatus.msg"
```

následne môžeme buildovať

build pre konkretny package
```
colcon build --packages-select <package_name>
```

nakoľko posledná pripravená knižnica potrebuje funkčné interfaces, musíme package zbuildovat predtým ako ideme buildovat dalsi a nasourcovať si ho
```
source install/setup.bash
```
následne môžeme pridať do zložky druhú knižnicu a zbuildovat spolu klasicky

## Volanie servisov 

volanie takeoff
```
ros2 service call /takeff std_srvs/srv/Trigger "{}"
```

volanie land
```
ros2 service call /land std_srvs/srv/Trigger "{}"
```

volanie zmeny let modu na GUIDED
```
ros2 service call /set_mode std_srvs/srv/Trigger "{}"
```

volanie letu na bod
```
ros2 service call /fly_to_point bpl_interfaces/srv/Position "{latitude: -35.363261, longtitude: 149.1652373, altitude: 603.5586}"
```
