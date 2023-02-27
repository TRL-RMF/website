# Emergency Requirements & Behaviors

In the event of fire incidents happen, some of the building facilities will be set to special mode as part of fire evacuation plan and will not respond to any user requests until fire alarm being released. At the same time, robots are highly encouraged to be evacuated to dedicated locations as well so that damage evaluation can be easily done at later time. Having this in mind, RMF is integrated with a capability to instruct robots to pause recent unfinished tasks and send them to nearest parking spots which may be at another floor.

## Detailed Requirements

1. Whenever lobby service lift is under emergency alarm mode, RMF must be notified through a ROS topic.
1. RMF must then instruct robots to pause recent unfinished tasks and send them to nearest parking spots.
1. Robots must stay put at the parking spots until emergency alarm being deactivated.
1. Once emergency alarm is deactivated, RMF must be notified again.
1. RMF must release robots and allows them to resume unfinished tasks.

## Behaviors
During the UAT test, all emergency requirements mentioned above were satisfied and some videos have been taken for evidence purpose.

1 of the videos
<br>
<video src="../images/fire_alarm_test.webm" controls="controls" style="max-width: 730px;">
</video>



