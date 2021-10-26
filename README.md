# test-rmw

Testing RMW https://github.com/ros2/rmw_fastrtps for ROS2 dashing.

# Build
```
cd ~/ros2_ws/src
colcon build 
```

#### Example

The following example configures Fast DDS to publish synchronously, and to have a pre-allocated history that can be expanded whenever it gets filled.

1. Create a Fast DDS XML file with:

    ```XML
    <?xml version="1.0" encoding="UTF-8"?>
    <dds xmlns="http://www.eprosima.com/XMLSchemas/fastRTPS_Profiles">
        <profiles>
            <!-- Default publisher profile -->
            <publisher profile_name="default publisher profile" is_default_profile="true">
                <qos>
                    <publishMode>
                        <kind>SYNCHRONOUS</kind>
                    </publishMode>
                </qos>
                <historyMemoryPolicy>PREALLOCATED_WITH_REALLOC</historyMemoryPolicy>
            </publisher>

            <!-- Publisher profile for topic helloworld -->
            <publisher profile_name="helloworld">
                <qos>
                    <publishMode>
                        <kind>SYNCHRONOUS</kind>
                    </publishMode>
                </qos>
            </publisher>

            <!-- Request subscriber profile for services -->
            <subscriber profile_name="service">
                <historyMemoryPolicy>PREALLOCATED_WITH_REALLOC</historyMemoryPolicy>
            </subscriber>

            <!-- Request publisher profile for clients -->
            <publisher profile_name="client">
                <qos>
                    <publishMode>
                        <kind>ASYNCHRONOUS</kind>
                    </publishMode>
                </qos>
            </publisher>

            <!-- Request subscriber profile for server of service "add_two_ints" -->
            <subscriber profile_name="rq/add_two_intsRequest">
                <historyMemoryPolicy>PREALLOCATED_WITH_REALLOC</historyMemoryPolicy>
            </subscriber>

            <!-- Reply subscriber profile for client of service "add_two_ints" -->
            <subscriber profile_name="rr/add_two_intsReply">
                <historyMemoryPolicy>PREALLOCATED_WITH_REALLOC</historyMemoryPolicy>
            </subscriber>
        </profiles>
    </dds>
    ```

1. Run the talker/listener ROS 2 demo:
    1. In one terminal

        ```bash
        FASTRTPS_DEFAULT_PROFILES_FILE=<path_to_xml_file> RMW_FASTRTPS_USE_QOS_FROM_XML=1 RMW_IMPLEMENTATION=rmw_fastrtps_cpp ros2 run demo_nodes_cpp talker
        ```

    1. In another terminal

        ```bash
        FASTRTPS_DEFAULT_PROFILES_FILE=<path_to_xml_file> RMW_FASTRTPS_USE_QOS_FROM_XML=1 RMW_IMPLEMENTATION=rmw_fastrtps_cpp ros2 run demo_nodes_cpp listener
        ```
