VESC Canbus Communication
=========================

-  A CAN Bus frame can contain a maximum of 8 bytes of data.
-  Each CAN node has a unique ID, the ID determines the priority (ID 0
   is most dominant).
-  CAN messages from VESC use an extended ID (EID), containing the
   COMMAND and CONTROLLER\_ID.

CANBus Control
--------------

These commands can be sent to a VESC node to control the motor and
request status.

+--------------------------+-----+-----------------+-------------+-------------+-------+
| Command                  | ID  | Data            | Data Length | Data Type   | Units |
+==========================+=====+=================+=============+=============+=======+
| CAN\_PACKET\_SET\_DUTY   | 0   | Motor Duty      | 32-bit/4-by | Signed      | Thous |
|                          |     | Cycle           | te          | Integer     | andth |
|                          |     |                 |             |             | s     |
|                          |     |                 |             |             | of    |
|                          |     |                 |             |             | perce |
|                          |     |                 |             |             | nt    |
|                          |     |                 |             |             | (5000 |
|                          |     |                 |             |             | 0     |
|                          |     |                 |             |             | -->   |
|                          |     |                 |             |             | 50%)  |
+--------------------------+-----+-----------------+-------------+-------------+-------+
| CAN\_PACKET\_SET\_CURREN | 1   | Motor Current   | 32-bit/4-by | Signed      | mAh   |
| T                        |     |                 | te          | Integer     |       |
+--------------------------+-----+-----------------+-------------+-------------+-------+
| CAN\_PACKET\_SET\_CURREN | 2   | Motor Brake     | 32-bit/4-by | Signed      | mAh   |
| T\_BRAKE                 |     | Current         | te          | Integer     |       |
+--------------------------+-----+-----------------+-------------+-------------+-------+
| CAN\_PACKET\_SET\_RPM    | 3   | Motor RPM       | 32-bit/4-by | Signed      | ERPM  |
|                          |     |                 | te          | Integer     |       |
+--------------------------+-----+-----------------+-------------+-------------+-------+
| CAN\_PACKET\_SET\_POS    | 4   | Motor Position  |             |             |       |
+--------------------------+-----+-----------------+-------------+-------------+-------+
| CAN\_PACKET\_FILL\_RX\_B | 5   |                 |             |             |       |
| UFFER                    |     |                 |             |             |       |
+--------------------------+-----+-----------------+-------------+-------------+-------+
| CAN\_PACKET\_FILL\_RX\_B | 6   |                 |             |             |       |
| UFFER\_LONG              |     |                 |             |             |       |
+--------------------------+-----+-----------------+-------------+-------------+-------+
| CAN\_PACKET\_PROCESS\_RX | 7   |                 |             |             |       |
| \_BUFFER                 |     |                 |             |             |       |
+--------------------------+-----+-----------------+-------------+-------------+-------+
| CAN\_PACKET\_PROCESS\_SH | 8   |                 |             |             |       |
| ORT\_BUFFER              |     |                 |             |             |       |
+--------------------------+-----+-----------------+-------------+-------------+-------+
| CAN\_PACKET\_STATUS      | 9   | Request status  | N/A         |             |       |
+--------------------------+-----+-----------------+-------------+-------------+-------+
| CAN\_PACKET\_SET\_CURREN | 10  |                 |             |             |       |
| T\_REL                   |     |                 |             |             |       |
+--------------------------+-----+-----------------+-------------+-------------+-------+
| CAN\_PACKET\_SET\_CURREN | 11  |                 |             |             |       |
| T\_BRAKE\_REL            |     |                 |             |             |       |
+--------------------------+-----+-----------------+-------------+-------------+-------+

-  Motor position requires an encoder to be present.

CanBus Status
-------------

The VESC can be configured to send status updates at a set frequency.

Triforce Network Topology
-------------------------

```
        +----------------+     +----------------+     +----------------+
        | Weapon Motor 1 |     | Weapon Motor 1 |     | Weapon Motor 1 |
        | CAN ID 3       |     | CAN ID 4       |     | CAN ID 5       |
        +-----+----+-----+     +-----+----+-----+     +-----+----+-----+
              |    |                 |    |                 |    |         +-------------------+
              |    |                 |    |                 |    |         | Fight Controller  |
              |    |                 |    |                 |    |         |                   |
      +-------+------------+---------+-----------------+----+---------------------+            |
     +-+           |       |              |            |         |         |     +-+           |
120R | |           |       |              |            |         |         |     | | 120R      |
     +-+           |       |              |            |         |         |     +-+           |
      +------------+------------+---------+-------+--------------+----------------+            |
                           |    |                 |    |                   |                   |
                           |    |                 |    |                   |     CAN ID 0      |
                     +-----+----+----+      +-----+----+----+              +-------------------+
                     | Drive Motor 1 |      | Drive Motor 1 |
                     | CAN ID 1      |      | CAN ID 2      |
                     +---------------+      +---------------+

```
