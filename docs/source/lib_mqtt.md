# PIKA-MQTT libary

### `__init__()`

**introduce：**

Instantiate one MQTT Client

**args：**

| serial number | function      | types of variables |        comment            |
| ---- | --------- | -------- | ---------------------- |
| 1    | ip        | string   |required                   |
| 2    | port      | int      |This parameter is optional. The default value is 1883  |
| 3    | clientID  | string   |This parameter is optional. The default value is mac of address  |
| 4    | username  | string   |This parameter is optional. The default value is null         |
| 5    | password  | string   |This parameter is optional. The default value is null         |
| 6    | version   | string   |This parameter is optional. The default value is MQTT-3.1.1 |
| 7    | ca        | string   |This parameter is not required and is empty by default         |
| 8    | keepalive | int      |This parameter is not required. The default value is 60 seconds    |

**returned value：**

|    function  | types of variables |
| -------- | -------- |
| mqtt event | MQTT     |

**give a typical example：**

```python
# Minimalist creation
c = MQTT("broker-cn.emqx.io")
# Create a custom port
c = MQTT("broker-cn.emqx.io", 1111)
# Create a custom clientID
c = MQTT("broker-cn.emqx.io", clientID="pikascript")
# Requires the creation of a user name and password
c = MQTT("broker-cn.emqx.io", username="pikascript123", password="123456")
# The creation of an encrypted mqtt
c = MQTT("broker-cn.emqx.io", 8883, username="pikascript123", password="123456", ca=open(cert_file).read())
```

------

### `setClientID()`

**introduce:**

Setting clientID overrides the parameters at instantiation time
**parameter:**

| serial number | function     | types of variables | comment |
| ---- | -------- | -------- | ---- |
| 1    | clientid | string   | required |

****

| function                  | types of variables |
| --------------------- | -------- |
| OK:true  fail:false | bool     |

**give a typical example：**

```python
c.setClientID("pikascript")
```

------

### `setUsername()`

**introduce：**

Setting usrname overrides the instantiation parameters

**parameter：**

| serial number | function     | types of variables | cointroducemment |
| ---- | -------- | -------- | ---- |
| 1    | username | string   | required |

**returned value：**

| function                  | types of variables |
| --------------------- | -------- |
| OK:true  fail:false | bool     |

**give a typical example：**

```python
c.setUsername("pikascript")
```

------

### `setPassword()`

**introduce：**

Setting password overrides the parameter used during instantiation

**parameter：**

| serial number | function     | types of variables | comment |
| ---- | -------- | -------- | ---- |
| 1    | password | string   | required |

**returned value：**

| function                  | types of variables |
| --------------------- | -------- |
| OK:true  fail:false | bool     |

**give a typical example：**

```python
c.setPassword("pikascript")
```

------

### `setVersion()`

**introduce：**

Setting the mqtt version overrides the parameter at instantiation time

**parameter：**

| serial number | function    | types of variables | comment |
| ---- | ------- | -------- | ---- |
| 1    | version | string   | required |

**returned value：**

| function                  | types of variables |
| --------------------- | -------- |
| OK:true  fail:false | bool     |

**give a typical example：**

```python
# choosable "3.1" "3.1.1"
c.setVersion("3.1")
```

------

### `setCa()`

**introduce：**

Setting the ssl certificate overrides the parameter during instantiation
Once this parameter is in effect, an ssl connection is forced

**parameter：**

| serial number | function | types of variables | comment |
| ---- | ---- | -------- | ---- |
| 1    | ca   | string   | required |

**returned value：**

| function                  | types of variables |
| --------------------- | -------- |
| OK:true  fail:false | bool     |

**give a typical example：**

```python
c.setCa(open(cert_file).read())
```

------

### `setKeepAlive()`

**introduce：**

Setting the keepalive time overrides the parameter at instantiation time

**parameter：**

| serial number | function | types of variables | comment |
| ---- | ---- | -------- | ---- |
| 1    | time | int      | required |

**returned value：**

| function                  | types of variables |
| --------------------- | -------- |
| OK:true  fail:false | bool     |

**give a typical example：**

```python
# cgs unit
c.setKeepAlive(120)
```

------

### `setWill()`

**introduce：**

testamentary

**parameter：**

| serial number | function    | types of variables | comment              |
| ---- | ------- | -------- | ----------------- |
| 1    | topic   | string   | required              |
| 2    | payload | string   | required              |
| 3    | qos     | int      | No required，defaultQOS0  |
| 4    | retain  | bool     | No required，defaultFalse |

**returned value：**

| function                  | types of variables |
| --------------------- | -------- |
| OK:true  fail:false | bool     |

**give a typical example：**

```python
c.setWill("/device/will", "{"name":"pikascript"}")
```

------

### `setDisconnectHandler()`

**introduce：**

Set the disconnection callback

**parameter：**

| serial number | function     | types of variables | comment          |
| ---- | -------- | -------- | ------------- |
| 1    | callback | any      | required callback function |

**returned value：**

| function                  | types of variables |
| --------------------- | -------- |
| OK:true  fail:false | bool     |

**give a typical example：**

```python
def disconnect_cb():
    print("mqtt disconnect")

c.setDisconnectHandler(disconnect_cb)
```

------

### `connect()`

**introduce：**

connect to server

**parameter：**

null

**returned value：**

| function                | types of variables |
| ------------------- | -------- |
| OK:0   | int  Error code: attached one    |

**give a typical example：**

```python
c.connect()
```

------

### `disconnect()`

**introduce：**

Disconnect server

**parameter：**

null

**returned value：**

| function                | types of variables |
| ------------------- | -------- |
| OK:0  int  Error code: attached one | int      |

**give a typical example：**

```python
c.disconnect()
```

------

### `subscribe()`

**introduce：**

Subscribe to a subject

**parameter：**

| serial number | function     | types of variables | comment             |
| ---- | -------- | -------- | ---------------- |
| 1    | topic    | string   | required             |
| 2    | callback | any      | required，回调函数   |
| 3    | qos      | int      | 非required，defaultQOS0 |

**returned value：**

| function                | types of variables |
| ------------------- | -------- |
| OK:0  int  Error code: attached one | int      |

**give a typical example：**

```python
def sub_cb(evt):
    print(evt.msg, evt.topic)

c.subscribe("/topic/sub", sub_cb)
```

------

### `unsubscribe()`

**introduce：**

Unsubscribe to a topic

**parameter：**

| serial number | function  | types of variables | comment |
| ---- | ----- | -------- | ---- |
| 1    | topic | string   | required |

**returned value**

| function                | types of variables |
| ------------------- | -------- |
| OK:0   | int Error code: attached one     |

**give a typical example**

```python
c.unsubscribe("/topic/sub")
```

------

### `listSubscribeTopics()`

**introduce**

Lists the topics to which you are currently subscribed

**parameter:**

null

**returned value**

| function     | types of variables |
| -------- | -------- |
| subject bibliography | tuple    |

**example:**

```python
t = c.listSubscribeTopics()
print(t)
```

------

### `publish()`

**introduce:**

publish the message

**parameters:**

| serial number | function    | types of variables | comment             |
| ---- | ------- | -------- | ---------------- |
| 1    | topic   | string   | required             |
| 2    | payload | string   | required             |
| 3    | qos     | int      | 非required，defaultQOS0 |

**return values：**

| function                | types of variables |
| ------------------- | -------- |
| OK:0   | int Error code: attached one   |

**example：**

```python
c.publish("/topic/pub", 0, "{"msg":"hello pikascript"}")
```

------

### Attachment 1: Error code

| error code | explain                       | comment                                    |
| ------ | -------------------------- | ---------------------------------------- |
| 0      | STATE_SUCCESS              | OK                                     |
| 1      | STATE_FAIL_UNKONW          | unknown cause of failure               |
| 2      | STATE_FAIL_BAD_NAME_PASSWD | Failed the user name and password are incorrect     |
| 3      | STATE_FAIL_BAD_CLINETID    | Falid clientid error                       |
| 4      | STATE_FAIL_BAD_PROTOCOL    | Falid the mqtt protocol version is incorrect   |
| 5      | STATE_FAIL_BAD_RCODE       | Falid to subsribe or ubsubscribe the server returns the operation falid |
| 6      | STATE_FAIL_PERMISSION      |Permissions are unreachable when a subscription or publication fails       |

------

### Second:Comprehensive example

```python
# coding=utf-8

def sub_test1_cb(evt):
    print("sub test1 message:", evt.msg)


def sub_test2_cb(evt):
    print("sub test2 message:", evt.msg)


def sub_test3_cb(evt):
    print("sub test3 message:", evt.msg)
    
    
def disconnect_cb():
    print("mqtt disconnect")

    
def test():
    # MQTTS
    c = MQTT("broker-cn.emqx.io", 8883,  clientID="pikaone", username="pikascript123", password="123456", ca=open("/ca.crt").read())
    
    # Test a mentary
    c.setWill("/device/will", "{"name":"pikascript"}", 0, True)
    
	# Set the dis connection callback
	c.setDisconnectHandler(disconnect_cb)
    
    # connect to server
    result = c.connect()
    if result == 0:
        print("connect success!")
    else:
        print("connect faild id={}".format(result))
        return
    
    # Subscribe to a subject
    c.subscribe("/pikascript/test1", sub_test1_cb, 0)
    c.subscribe("/pikascript/test2", sub_test2_cb, 1)
    c.subscribe("/pikascript/test3", sub_test3_cb, 2)

    print("list subscribe topics:")
    print(c.listSubscribeTopics())

    print("start publish")

    # Send topic message
    c.publish("/pikascript/test1", "{'msg'}:'hello from test1'", 0)
    c.publish("/pikascript/test2", "{'msg'}:'hello from test2'", 1)
    c.publish("/pikascript/test3", "{'msg'}:'hello from test3'", 2)

    print("end publish")

    # Discount
    result = c.disconnect()
    if result == 0:
        print("disconnect success!")
    else:
        print("disconnect faild id={}".format(result))
        return
    
# run
test()
```

flow chart 
![mqtt](assets/d03165ee_5512529.png "pika-mqtt.png")

