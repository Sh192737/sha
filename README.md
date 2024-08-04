 Manipulating the `turtlesim` Package in ROS Noetic and ROS2 Foxy

This task aims to use the `turtlesim` package in both ROS Noetic and ROS2 Foxy to control a graphical turtle. The objective is to learn the basics of controlling virtual robots using the ROS system.

Tools and Requirements
1. **Operating System**: Ubuntu 20.04
2. **ROS Noetic**
3. **ROS2 Foxy**
4. **`turtlesim` Package**

 Part 1: Manipulating `turtlesim` with ROS Noetic

 Steps:

1. **Install ROS Noetic**:
   Follow the official installation guide for ROS Noetic: [Installation Guide](http://wiki.ros.org/noetic/Installation/Ubuntu).

2. **Install the `turtlesim` Package**:
   ```bash
   sudo apt-get update
   sudo apt-get install ros-noetic-turtlesim
   ```

3. **Run the `turtlesim` Node**:
   - In the first terminal, run:
     ```bash
     roscore
     ```
   - In a second terminal, run:
     ```bash
     rosrun turtlesim turtlesim_node
     ```

4. **Control the Turtle**:
   - Open a new terminal and run:
     ```bash
     rosrun turtlesim turtle_teleop_key
     ```
   - Use the arrow keys to control the turtle.

5. **Writing a Simple Publisher and Subscriber**:
   Create a Python script to publish commands to the turtle. Below is an example:

   ```python
   # publisher.py
   import rospy
   from geometry_msgs.msg import Twist

   def move_turtle():
       rospy.init_node('move_turtle', anonymous=True)
       pub = rospy.Publisher('/turtle1/cmd_vel', Twist, queue_size=10)
       rate = rospy.Rate(10)  # 10hz
       
       while not rospy.is_shutdown():
           vel_msg = Twist()
           vel_msg.linear.x = 2.0
           vel_msg.angular.z = 1.8
           pub.publish(vel_msg)
           rate.sleep()

   if __name__ == '__main__':
       try:
           move_turtle()
       except rospy.ROSInterruptException:
           pass
   ```

   Run the script in a terminal:
   ```bash
   rosrun your_package_name publisher.py
   ```

 Part 2: Manipulating `turtlesim` with ROS2 Foxy

 Steps:

1. **Install ROS2 Foxy**:
   Follow the official installation guide for ROS2 Foxy: [Installation Guide](https://docs.ros.org/en/foxy/Installation.html).

2. **Install the `turtlesim` Package**:
   ```bash
   sudo apt update
   sudo apt install ros-foxy-turtlesim
   ```

3. **Run the `turtlesim` Node**:
   Open a terminal and run:
   ```bash
   source /opt/ros/foxy/setup.bash
   ros2 run turtlesim turtlesim_node
   ```

4. **Control the Turtle**:
   Open a new terminal and run:
   ```bash
   source /opt/ros/foxy/setup.bash
   ros2 run turtlesim turtle_teleop_key
   ```
   Use the arrow keys to control the turtle.

5. **Writing a Simple Publisher and Subscriber**:
   Create a Python script to publish commands to the turtle. Below is an example:

   ```python
   # publisher.py
   import rclpy
   from rclpy.node import Node
   from geometry_msgs.msg import Twist

   class TurtleController(Node):
       def __init__(self):
           super().__init__('turtle_controller')
           self.publisher_ = self.create_publisher(Twist, 'turtle1/cmd_vel', 10)
           timer_period = 0.5  # seconds
           self.timer = self.create_timer(timer_period, self.timer_callback)

       def timer_callback(self):
           msg = Twist()
           msg.linear.x = 2.0
           msg.angular.z = 1.8
           self.publisher_.publish(msg)

   def main(args=None):
       rclpy.init(args=args)
       turtle_controller = TurtleController()
       rclpy.spin(turtle_controller)
       turtle_controller.destroy_node()
       rclpy.shutdown()

   if __name__ == '__main__':
       main()
   ```

   Run the script in a terminal:
   ```bash
   ros2 run your_package_name publisher.py
   ```
