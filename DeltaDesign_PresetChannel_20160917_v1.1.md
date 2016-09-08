# 1. introduction
Delta save five preset channels. The user can select it to play using button on speaker or Libratone-app.
The document has several part to introduce it:

- Default preset-channels saved in speaker.
- Update a preset-channel.
- Get preset-channels information.
- App play a channel.
- Speaker select and play a channel.
- App get the information of the playing channel.


# 2. Default preset-channels saved in speaker
When first boot after factory reset, the default preset-channels is empty. Speaker will general 5 default preset-channels according the **region**.

## 1. region = "CN"


The default preset-channels is const:

	{
	   "bitmap" : 31,
	   "channel_1" : {
		  "channel_id" : 1,
		  "channel_identity" : "0",
		  "channel_name" : "私人",
		  "channel_type" : "doubanfm"
	   },
	   "channel_2" : {
		  "channel_id" : 2,
		  "channel_identity" : "28250",
		  "channel_name" : "李健系",
		  "channel_type" : "doubanfm"
	   },
	   "channel_3" : {
		  "channel_id" : 3,
		  "channel_identity" : "3637695",
		  "channel_name" : "Easy",
		  "channel_type" : "doubanfm"
	   },
	   "channel_4" : {
		  "channel_id" : 4,
		  "channel_identity" : "222223",
		  "channel_name" : "内瓦儿歌",
		  "channel_type" : "xmly"
	   },
	   "channel_5" : {
		  "channel_id" : 5,
		  "channel_identity" : "61",
		  "channel_name" : "郭德纲于谦相声全集",
		  "channel_type" : "xmly"
	   }
	}

## 2. region = "EU"/"NA"/"JP"
The default preset-channels is vTuner's most popular 5 channels in current location. The speaker will access the vTuner's server and get them and set to speaker. Before that, the five default channels is saved in speaker.

### Default channels
	{
	   "bitmap" : 31,
	   "channel_1" : {
		  "channel_id" : 1,
		  "channel_identity" : "26128",
		  "channel_name" : "BBC Radio 6 Music",
		  "channel_type" : "vtuner"
	   },
	   "channel_2" : {
		  "channel_id" : 2,
		  "channel_identity" : "3158",
		  "channel_name" : "BBC Radio 4",
		  "channel_type" : "vtuner"
	   },
	   "channel_3" : {
		  "channel_id" : 3,
		  "channel_identity" : "13417",
		  "channel_name" : "Radio Paradise",
		  "channel_type" : "vtuner"
	   },
	   "channel_4" : {
		  "channel_id" : 4,
		  "channel_identity" : "36322",
		  "channel_name" : "Bossa Jazz Brasil",
		  "channel_type" : "vtuner"
	   },
	   "channel_5" : {
		  "channel_id" : 5,
		  "channel_identity" : "24570",
		  "channel_name" : "Chinese Music World",
		  "channel_type" : "vtuner"
	   }
	}

### update channels from vtuner server
The channels from vtuner server is dynamic. The vtuner will return the list according to the request ip address. But once the channels are got successfully, the speaker will not change even if the ip address has been changed.

	{
	   "bitmap" : 31,
	   "channel_1" : {
	      "channel_id" : 1,
	      "channel_identity" : "1151",
	      "channel_name" : "RTHK Radio 1 92.6 FM",
	      "channel_type" : "vtuner"
	   },
	   "channel_2" : {
	      "channel_id" : 2,
	      "channel_identity" : "1152",
	      "channel_name" : "RTHK Radio 2 94.8 FM",
	      "channel_type" : "vtuner"
	   },
	   "channel_3" : {
	      "channel_id" : 3,
	      "channel_identity" : "4205",
	      "channel_name" : "RTHK Radio 5 783 AM",
	      "channel_type" : "vtuner"
	   },
	   "channel_4" : {
	      "channel_id" : 4,
	      "channel_identity" : "832",
	      "channel_name" : "RTHK Mandarin 621 AM",
	      "channel_type" : "vtuner"
	   },
	   "channel_5" : {
	      "channel_id" : 5,
	      "channel_identity" : "1153",
	      "channel_name" : "RTHK Radio 3 97.9 FM",
	      "channel_type" : "vtuner"
	   }
	}


# 3. Update a preset-channel



## 1. Common

### MB114H\_SET\_PRECHANNEL

| **Unique Remote Id** | **CMD Type** | **MB Id** | **CMD Status** | **CRC** | **Data Length** | **Data** |
| --- | --- | --- | --- | --- | --- | --- |
| 0xAAAA | SET | 0x114 | NA | 0 | Length of Data Field | Channel information |

### **Channel information**

Example string of Json format:

	{
	  "channel_identity": "",
	  "channel_name": "",
	  "channel_type": "",
	  "channel_id": , 
	  "username": "",
	  "password": "",
	  "token":"",
	  "user_blob": "",
	}


| Json Field |Description | Choice |
| --- | --- | --- |
| **channel\_id** | 1-5, delta can save 5 preset channels | **required** |
| **channel\_identity** | The music list&#39;s id of Music provider |**required** |
| **channel\_name** |The music list&#39;s name |**required** |
| **channel\_type** |  Music provider. |**required** |
| **username** | DoubanFm or spotify username |**required** when **channel_type** is **doubanfm**|
| **password** | DoubanFm password( [encryption](javascript:void(0);)) |**required** when **channel_type** is **doubanfm**|
| **token** | DoubanFm access token |**optional** when **channel_type** is **doubanfm**.|
| **user_blob** | spotify's user blob, used to spotify sign in | just for spotify <br> `the blob is complex, detail introduction later` |


#### channel_type supported
- vtuner
- doubanfm
- xmly
- audiogum-spotify
- spotify



_MB114H\_SET\_PRECHANNEL\_RESP_

| **Unique Remote Id** | **CMD Type** | **MB Id** | **CMD Status** | **CRC** | **Data Length** | **Data** |
| --- | --- | --- | --- | --- | --- | --- |
| 0xAAAA | SET | 0x114 | 1(success) | NA |   |   |

## 2. Audigum

### 1. Spotify

- example:

	    {	
			"channel_identity": " f4dc137e13c64276971f47aa9bc3754e",	
			"channel_name": "Starred",	
			"channel_type": "audiogum-spotify",				
			"channel_id": 5,
		}


	- **channel_identity**:  audiogum's playable
	- **channel_name**: spotify playlist’s name
	- **channel_type**: audiogum-spotify
	- **channel_id**: 1-5

## 2. Spotify

- sequence

![ScrumModel](https://github.com/leino11121/markdown_picture/blob/master/Delta_PresetChannel_Spotify_blob.png?raw=true)


- example:

	    {	
			"channel_identity": "xxxx",	
			"channel_name": "Starred",	
			"channel_type": "spotify",	
			"user-blob": "xxxx",	
			"channel_id": 5,
		}


	- **channel_identity**:  playlist blob
	- **channel_name**: spotify playlist’s name
	- **channel_type**: spotify
	- **channel_id**: 1-5

## 3. Douban FM

- example:

	    {	
			"channel_identity": " 64 ",	
			"channel_name": "私人",	
			"channel_type": "doubanfm",	
			"access_token": "xxxx",	
			"channel_id": 5,	
			"username": "doubanfm's username",
			"password": "encrypted password"
		}

	- **channel_identity**:  doubanfm's channel id
	- **channel_name**: doubanfm's name
	- **channel_type**: doubanfm
	- **access_token**: doubanfm’s token
	- **channel_id**: 1-5
	- **username**: doubanfm’s username
	- **password**: user's encrypted password

## 4. vTuner

- example:

	    {	
			"channel_identity": " 60867 ",	
			"channel_name": "Beijing Radio Music FM 97.4",	
			"channel_type": "vtuner",
			"channel_id": 5
		}

	- **channel_identity**:  channel id
	- **channel_name**:  channel name
	- **channel_type**: vtuner
	- **channel_id**: 1-5

## 5. ximalaya

- example:

	    {	
			"channel_identity": " 1234 ",	
			"channel_name": "曹云金相声合集",	
			"channel_type": "xmly",
			"channel_id": 5
		}

	- **channel_identity**:  channel id
	- **channel_name**:  channel name
	- **channel_type**: xmly
	- **channel_id**: 1-5

# 4. Get preset-channels information

get all preset-channel information from app

## MB113H\_GET\_ALL\_PRE\_CHANNEL

| **Unique Remote Id** | **CMD Type** | **MB Id** | **CMD Status** | **CRC** | **Data Length** | **Data** |
| --- | --- | --- | --- | --- | --- | --- |
| 0xAAAA | GET | 0x113 | NA | NA | NA |   |

### example:
	{
	   "bitmap" : 31,
	   "channel_1" : {
	      "channel_id" : 1,
	      "channel_identity" : "1151",
	      "channel_name" : "RTHK Radio 1 92.6 FM",
	      "channel_type" : "vtuner"
	   },
	   "channel_2" : {	
			"channel_identity": " 64 ",	
			"channel_name": "私人",	
			"channel_type": "doubanfm",	
			"access_token": "xxxx",	
			"channel_id": 5,	
			"username": "doubanfm's username",
			"password": "encrypted password"
		},
	   "channel_3" : {	
			"channel_identity": " 1234 ",	
			"channel_name": "曹云金相声合集",	
			"channel_type": "xmly",
			"channel_id": 5
		},
	   "channel_4" : {	
			"channel_identity": " f4dc137e13c64276971f47aa9bc3754e",	
			"channel_name": "Starred",	
			"channel_type": "audiogum-spotify",	
			"channel_id": 5,	
		},
	   "channel_5" : {
	      "channel_id" : 5,
	      "channel_identity" : "xxxx",
	      "channel_name" : "started",
	      "channel_type" : "spotify",
	      "user-blob" : "xxxx",
	   }
	}


## MB113H\_GET\_ALL\_PRE\_CHANNEL\_RESP

| **Unique Remote Id** | **CMD Type** | **MB Id** | **CMD Status** | **CRC** | **Data Length** | **Data** |
| --- | --- | --- | --- | --- | --- | --- |
| 0xAAAA | GET | 0x113 | NA | 0 | Length of Data Field | Channel information |


# 5. App play a channel

## MB115H\_SET\_PRECH\_PLAY\_APP

| **Unique Remote Id** | **CMD Type** | **MB Id** | **CMD Status** | **CRC** | **Data Length** | **Data** |
| --- | --- | --- | --- | --- | --- | --- |
| 0xAAAA | SET | 0x115 | NA | NA | Length of Data Field | Channel information |

### **Channel information**


String of Json format:

- Type1: 

Push the playlist to speaker, but is not belong to the preset channels.

	{
	  "play_identity": "25900",
	  "play_title": "John Surman ",
	  "play_type": "doubanfm",
	  "token": "7e5a8dabe821660e7b0fa903b864322a"
	}
- Type2:

play the channel 2.

	{
	  "play_identity": "2",
	  "play_type": "channel",
	}


| **Json Field** | Type1 | Type2 |
| --- | --- | --- |
| **play\_identity** | The music list&#39;s id of Music provider | 1-5, delta can save 5 preset channels |
| **play\_title** | The music list&#39;s name |   |
| **play\_type** | Music provider, DoubanFm(doubanfm). | Fix to &quot;channel&quot; |
| **token** | Empty for not douban |   |

## MB115H\_SET\_PRECH\_PLAY\_APP\_RESP

| **Unique Remote Id** | **CMD Type** | **MB Id** | **CMD Status** | **CRC** | **Data Length** | **Data** |
| --- | --- | --- | --- | --- | --- | --- |
| 0xAAAA | SET | 0x115 | 1(success) | NA | NA |   |


# 6. Speaker select and play a channel
Use for mcu, TBD.

# 7. App get the information of the playing channel

## MB116H\_GET\_PRECH\_PLAY\_INFO

| **Unique Remote Id** | **CMD Type** | **MB Id** | **CMD Status** | **CRC** | **Data Length** | **Data** |
| --- | --- | --- | --- | --- | --- | --- |
| 0xAAAA | GET | 0x116 | NA | NA | NA |   |

## MB116H\_GET\_PRECH\_PLAY\_INFO\_RESP

| **Unique Remote Id** | **CMD Type** | **MB Id** | **CMD Status** | **CRC** | **Data Length** | **Data** |
| --- | --- | --- | --- | --- | --- | --- |
| 0xAAAA | GET | 0x116 | NA | NA | Length of Data Field | Playing information |

## **Playing**  **information**

String of Json format:

**Type1**

	{	
	  "play_identity": "3395255",
	  "play_subtitle": "Soldier",
	  "play_type": "doubanfm",
	  "play_title" : "棋子",	
	}

**Type2**

	{
	  "play_identity": "3",
	  "play_subtitle": " The Hug",
	  "play_type": " channel",
	  "play_title" : "Easy",
	}

| **Json** Field |  Type1 | Type2 |
| --- | --- |--- |
| **play\_identity** | The music list&#39;s id of Music provider | 1-5, delta can save 5 preset channels |
| **play\_subtitle** | Song&#39;s name | Song&#39;s name |
| **play\_type** | Music provider, vTuner(vtuner) Ximalaya(xmly) DoubanFm(doubanfm) for now. | Fix to &quot;channel&quot; |
| **play\_title** | The music list&#39;s name | The music list&#39;s name |