# kinou

実行環境:Ubuntu 18.04＋ROS-melodic
<br>
## 1.	Gazeboを用いたマニピュレータのシミュレーション
### 作成したマニピュレータのモデル動作確認

```
$ roslaunch sixdofarm sixdofarm.launch
```
```
$ rosrun rviz rviz
```
上記コマンドを実行するとrvizが立ち上がる
<br>

※ロボットモデルの表示
<br>
rviz左下”Add”ボタンをクリック，出現するウィンドウから”RobotModel”を追加
<br>
rviz画面左に表示されている”Fixed Frame”を”map”から”base”へ変更
<br>
→　rviz上に6自由度のマニピュレータが出現
<br>

※マニピュレータの動作
<br>
別ウィンドウで立ち上がっている”joint_state_publisher_gui”のバーを動かすことでマニピュレータの各関節を回転可能

### rvizでのロボットアームのモデル操作
```
$ roslaunch sixdofarm_moveit_config demo.launch
```
上記コマンドを実行すると6自由度のマニピュレータが立ち上がる
<br>
マニピュレータの先端についているボールをドラッグすることで姿勢が変更可能
<br>

また，rviz左下の”Plannning”タブにある”Plan”を押すことで，ボールをドラッグした位置までの姿勢までの移動計画を行う
<br>
”Execure”を押すと”Plan”した移動計画を実行する
<br>

※” Plannning& Execure”を押すことで計画と実行を同時に行うことが可能
<br>
※”MotionPlanning”の中の”Planned Path”の中にある”Loop Animation”のチェックを外すと一回だけアニメーションが流れるようになる
 
 
 
 
 
 
## 2.	Gazebo上の移動ロボットモデル操作確認
### Moveit! と gazebo の接続確認
Moveit! と gazebo の接続を確認するため，下記コマンドを実行する
```
$ roslaunch sixdofarm_moveit_config sixdofarm_moveit.launch
```
上記コマンドでrviz，gazebo，”joint_state_publisher_gui”が立ち上がる
<br>

1.に記述したように，rviz上の”Plannning”タブにある”Plan”を押すことで，変更後の姿勢への計画を行う
<br>
”Execure”を押すと”Plan”した移動計画を実行する
<br>

また，” Plannning& Execure”を押すことで計画と実行を同時に行うことが可能
<br>

以上よりgazebo上のマニピュレータも連動して動作することが確認できる



## 3.	移動ロボットのナビゲーション
### 移動ロボットがrviz，gazebo上で動作するかの確認

下記コマンドでrviz，gazebo上に移動ロボットを表示させる
```
$ roslaunch wheel_robot wheel_robot_control.launch
```

下記コマンドで表示させた移動ロボットを移動させること可能
```
$ rqt
```
rqtではバーを動かすことで移動ロボットの操作が可能
<br>

また，下記コマンドをcatkin_ws内で実行することで移動ロボットをキーボード操作で動かすことも可能
<br>
※コマンドを打ったワークスペースをアクティブにしていないと操作ができなくなるため注意
```
$ rosrun wheel_robot_control key_teleop.py
```

## 4.	移動ロボットで地図の作成
### 移動ロボットを用いた地図作成
下記コマンドで地図作成が可能である
```
$ roslaunch wheel_robot wheel_robot.launch
```
上記のコマンドでgazebo起動後，障害物となるオブジェクトを配置する必要がある
```
$ roslaunch wheel_robot_gmapping wheel_robot_gmapping.launch
```
```
$ rqt
```
```
$ rosrun rviz rviz
```
上記のコマンドでrviz起動後，左下の”Add”ボタンをクリックし，出現するウィンドウから”RobotModel”，”Map”を追加
<br>
その後，”Map”タブ内のTopicを”/map”と設定
<br>
なお，3.同様，下記コマンドをcatkin_ws内で実行することで移動ロボットをキーボード操作で動かすことも可能
```
$ rosrun wheel_robot_control key_teleop.py
```
以上の動作を行った後，移動ロボットを動かすことで地図が作成される

### 地図の保存
地図の保存方法を下記コマンドで示す
<br>
※保存される地図は下記コマンドを実行したディレクトリに入る．また，”filename”は保存時のファイルネームであり，今回はtestとしている
```
$ rosrun map_server map_saver -f ”filename”
```

## 5.	差動二輪移動ロボットの確認
### 差動二輪ロボットのgazebo，rviz上での確認
下記コマンドで二輪ロボットが確認できる．ここでは，移動ロボットと部屋のオブジェクトが配置されている
```
$ roslaunch diff_mobile_robot diff_mobile_gazebo.launch
```
```
$ rosrun rviz rviz
```
rviz起動後，左下の”Add”ボタンをクリックし，出現するウィンドウから”RobotModel”を追加する
 

## 6.	差動２輪移動ロボットのナビゲーション
### 差動２輪移動ロボットを用いた地図作成
下記コマンドで地図作成が可能である
<br>
gazebo上には立ち上げ後，ここでは移動ロボットと部屋のオブジェクトが配置されている
```
$ roslaunch diff_mobile_robot diff_mobile_gazebo.launch
```
```
$ roslaunch wheel_robot_gmapping wheel_robot_gmapping.launch
```
```
$ rosrun rviz rviz]
```
上記のコマンドでrviz起動後，左下の”Add”ボタンをクリック
<br>
出現するウィンドウから”RobotModel”，”Map”を追加する．その後，”Map”タブ内のTopicを”/map”と設定する．
<br>
また，3.同様，下記コマンドのどちらかで差動二輪ロボットを動作させることが可能
```
$ rqt
```
または
```
$ rosrun wheel_robot_control key_teleop.py
```

以上の動作を行うと差動２輪移動ロボットで地図作成が可能となる

### 地図の保存
地図の保存方法を下記コマンドで示す
<br>
※保存される地図は下記コマンドを実行したディレクトリに入る．また，”filename”は保存時のファイルネームであり，今回はtestとしている
```
$ rosrun map_server map_saver -f ”filename”
```
### amclによる自己位置推定
下記のコマンドで，amclによる自己位置推定が可能であることを確認
```
$ roslaunch diff_mobile_robot diff_mobile_gazebo.launch
```
```
$ roslaunch diff_mobile_robot amcl.launch
```
```
$ rosrun rviz rviz
```
上記のコマンドでrviz起動後，左下の”Add”ボタンをクリックし，出現するウィンドウから”RobotModel”，”Map”，”PoseArray”を追加
<br>
その後，”Map”タブ内のTopicを”/map”に設定
<br>
また，”PoseArray” タブ内のTopicを”/particlecloud”に設定
<br>

ここで，rviz上の2D Nav Goalをクリックし，ロボットのゴールを決めると，ロボットが移動を開始する
<br>
このとき，amclによる自己位置推定の様子は”PoseArray”の赤い矢印で確認できる
<br>

### waypointの作成
次に，waypointｎの作成手順を示す
<br>
下記コマンドを実行することでwaypointが作成される
```
$ roslaunch diff_mobile_robot diff_mobile_gazebo.launch
```
```
$ roslaunch wheel_robot_gmapping wheel_robot_gmapping.launch
```
```
$ rosrun rviz rviz
```
下記のコマンドでwaypointｎの記録を行う
※保存されるwaypointｎは下記コマンドを実行したディレクトリに入る
```
$ rosrun odom_listener odom_listener
```

また，下記コマンドのどちらかで差動二輪ロボットを動作させることが可能
```
$ rqt
```
または
```
$ rosrun wheel_robot_control key_teleop.py
```

これで，odom_listenerを実行したディレクトリにwaypoints.yamlが生成される
<br>

今回は生成されたyamlファイルの名前をWaypoints2.yamlとした
<br>

次に行う，waypointを用いたナビゲーションのために保存したyamlファイルを以下の場所に移しておく
<br>
~/catkin_ws/src/wheel_robot_nav/waypoints
<br>

この時
<br>
~/catkin_ws/src/wheel_robot_nav/launch
<br>
内のwheel_robot_nav.launchの9行目を変更した名前(今回はWaypoints2.yaml)に変更しておくこと

### waypointを用いたナビゲーション

waypointを用いたナビゲーション手順を以下に示す
<br>
※使用するwaypointは先ほど作成したWaypoints2.yaml
<br>

ナビゲーションには下記コマンドを実行することで可能となる
```
$ roslaunch diff_mobile_robot diff_mobile_gazebo.launch
```
```
$ roslaunch wheel_robot_gmapping wheel_robot_gmapping.launch
```
```
$ rosrun rviz rviz
```
上記のコマンドでrviz起動後，左下の”Add”ボタンをクリックし，出現するウィンドウから”RobotModel”，”Map”を追加
<br>
その後，”Map”タブ内のTopicを”/map”と設定
```
$ roslaunch wheel_robot_nav wheel_robot_nav.launch
```

上記のコマンドを起動させることで，先ほど作成したWaypoints2.yamlに従ってロボットが移動を開始する
### waypointの確認
また，各waypointを確認するには，rviz左下の”Add”ボタンをクリックし，出現するウィンドウの”By topic”タブから” vizualization_marker”，を追加
<br>

同様に”By topic”タブから”/move_base/current_goal/Pose”を追加することでロボットの移動方向が視覚的に確認できる
<br>

更に，”/ move_base/NavfnROS/plan/Path”を追加するとゴールまでの経路計画が視覚的に確認できる
