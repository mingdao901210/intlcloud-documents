
In response to the requirements for copyright protection in the video industry, VOD has proposed a basic digital rights management (DRM) solution that uses the common AES encryption technology of HLS to encrypt video content for copyright protection. You can learn more about the DRM solution in the [VOD File Encryption Demo](http://demo.vod.qcloud.com/encryption/index.html) (username: test; password: 111111).

## Common Encryption Scheme of HLS

#### Encryption algorithm
The VOD system currently supports the encryption scheme (common AES encryption technology of HLS) as defined in [HLS](https://tools.ietf.org/html/draft-pantos-http-live-streaming-23), which uses AES-128 to encrypt video content.

#### Player adaptability
The video encryption solution of VOD supports all HLS players.

#### Limitations

- Only the 2017 version of APIs is supported: You can use [ProcessFile](#APIhttps://intl.cloud.tencent.com/document/product/266/9642) to initiate an encryption task and use GetTaskInfo <!--api (https://intl.cloud.tencent.com/document/product/266/11724)--> to query the encryption result.
- The new version of [ProcessMedia](#APIhttps://intL.cloud.tencent.com/document/product/266/33427) and [DescribeTaskDetail](#apihttps://intl.cloud.tencent.com/document/product/266/33431) APIs does not support HLS common encryption.

### Terminology

#### Key Management Service (KMS)
Tencent Cloud KMS is a security management service for creating, encrypting, and decrypting data keys. For more information, please see [KMS Overview](https://intl.cloud.tencent.com/product/kms).

#### Data Key (DK)
A DK is a key generated by KMS for symmetric encryption and decryption.

#### Encrypted Data Key (EDK)
An EDK is a DK encrypted by KMS, which can be publicly distributed. To exchange a DK with an EDK, the decryption API of KMS must be called.

### Overall architecture
![](https://main.qcloudimg.com/raw/53a2bbfe7d920e636c57574d266dc4ec.png)
The video encryption process is implemented through transcoding, which does not generate a new `fileId`. Compared with general transcoding scenarios, video encryption transcoding has the following differences:
- Encryption is performed during transcoding, i.e., the output video is encrypted.
- After encrypted transcoding is completed, if the video is played back through a VOD player, the playback URL of the source file will not be obtained.

### Preparations

#### Establishing KMS
KMS is mainly used to manage video keys. The following steps in the video encryption process need to interact with the KMS system:

1. Step II of generating a DK for video encryption as shown in the above architecture diagram. The DK and EDK are returned in this step. In the subsequent steps, the DK can be accessed by the VOD transcoding service, application backend, and authenticated end user, and the EDK can be distributed to any end user. However, to get the DK through the EDK, authentication must be performed by the application backend.
2. Step 4 of getting the DK through the EDK for video playback as shown in the above architecture diagram. After the end user is authenticated by the application, an API of KMS needs to be called to get the DK through the EDK as detailed in step 5.

In order to minimize your access costs, KMS is integrated in VOD and provides the simplest call APIs. In the entire video encryption solution, the only place where the application backend needs to interact with the KMS service is to get the decryption key (step 5 in the architecture diagram).

#### Building an authentication and key distribution service

For an encrypted video, only a client authenticated by the application backend can get the DK. Therefore, when an end user tries to get a key, authentication must be performed by the application backend. The business logic of this service is as follows:
1. If a client requests to get a DK with an EDK (step 4 in the architecture diagram), the requester needs to be authenticated.
2. If the authentication is passed, the DK will be obtained from the KMS system (step 5 in the architecture diagram) and returned to the client.

#### Suggestions
- As a DK always corresponds to a specific EDK, the correspondence between them can be cached (or even permanently stored) on the application backend to reduce the number of KMS calls (i.e., step 5 in the architecture diagram).
- An HTTP cache control parameter (such as Cache-Control) can be added to the response returned to the client by the application backend, so as to reduce the number of operations for the client to get the DK from the application backend (i.e., step 4 in the architecture diagram).
- If you need to support playback in browser, in order to avoid cross-domain issues, please make sure that the domain name used for the "authentication and key distribution service" is the same as that used on the "video playback page" (for example, if the domain name on the video playback page is `v.myvideo. com`, then the domain name for the key distribution service should also be `v.myvideo.com`).

#### Configuring a video encryption template
To ensure that the VOD backend can perform encryption operations correctly, you need to configure a video encryption template. For more information, please see [HLS Common Encryption Template](https://intl.cloud.tencent.com/document/product/266/33969).

### Business process

#### Video upload
You can upload an existing video file to the VOD platform by way of upload from server, client, console, recorder, or URL.

#### Video encryption

The main steps in video encryption include:

#### 1. The application backend initiates video encryption

Currently, you can initiate video encryption using the [ProcessFile](#APIhttps://intl.cloud.tencent.com/document/product/266/9642) API. Only HLS files can be encrypted for the time being.

The following example indicates:

- Transcode a video file. The target output templates of transcoding are 210, 220, 230, and 240, and transcoding from low bitrate to high bitrate is prohibited.
- The transcoding process is encrypted using encryption template 10.
- Event notification mode: An event notification will be sent after the entire event is completed.

<pre>
https://vod.api.qcloud.com/v2/index.php?Action=ProcessFile
&transcode.definition.0=210
&transcode.definition.1=220
&transcode.definition.3=230
&transcode.definition.4=240
&transcode.drm.definition=10
&amp;notifyMode=Finish
&COMMON_PARAMS
</pre>

#### 2. The VOD platform gets the encryption key
The VOD platform reads the key acquisition method based on the encryption parameter template specified by the caller. The end user gets the URL (e.g., `https://getkey.example.com`) of the decryption key and then gets the video encryption key (DK and EDK) from the KMS system.

#### 3. The VOD platform initiates encrypted video transcoding

When performing video encryption, the transcoding platform of VOD not only encrypts the target output file using the specified encryption algorithm and key, but also writes the URL for obtaining the decryption key into the video file. For example, for HLS, the URL will be written to the `EXT-X-KEY` tag in the M3U8 file. However, before writing, the transcoding platform will add the following three parameters to the `QueryString` of the URL:

- fileId: ID of the encrypted file.
2. keySource: The value of this parameter is always `VodBuildInKMS`, which means the KMS built in Tencent Cloud VOD.
3. edk: The EDK corresponding to the DK.

After the above parameters are added, the URL to be written into the target output video file may be as follows:

<pre>
https://getkey.example.com?fileId=123456&keySource=VodBuildInKMS&edk=abcdef
</pre>


This URL is also the one that the client needs to access when it tries to get the decryption key during video playback.

#### 4. The VOD platform initiates encryption and completes callback
When the status of the task flow that includes the encryption operation changes (or the task flow is completed), the VOD platform will trigger the [notification for task flow status change](https://intl.cloud.tencent.com/document/product/266/33953).

### Media asset management
After a video is encrypted, its encryption information can be obtained using the [GetVideoInfo](#APIhttps://intl.cloud.tencent.com/document/product/266/8586) API.
- The GetVideoInfo API will return the video playback addresses for the video ID under all transcoding specifications, including the playback address of the source file. As the source file is not encrypted, the application server can choose to filter out the playback address of the source file and only provide playback addresses of the encrypted video to the client.
- The `definition` parameter of the source file obtained by GetVideoInfo is 0, based on which the playback address of the source file can be filtered out.

## Video Playback Overview

Only authenticated end users can get a video decryption key. Therefore, how to authenticate end users is of top importance during playback.

In the playback process, the player can access the URL identified by the `EXT-X-KEY` tag in the M3U8 file to get the key. It needs to carry the end user's authentication information in this step, which can be passed to the application's authentication service in tow ways:

1. Append the end user's identity information as a parameter to the URL, and pass the URL to the application's authentication service. This scheme is suitable for all HLS players. For more information, please see [video playback scheme 1](#p1): passing authentication information through QueryString.
2. Bring the end user's identity information to the application's authentication service through a cookie. This method is more secure, but only suitable for players that carry cookies when accessing the URL identified by the `EXT-X-KEY` tag. For more information, please see [video playback scheme 2](#p2): passing authentication information through cookie.

### <span id="p1"></span>Video playback scheme 1

Video playback scheme 1: passing authentication information through QueryString. This scheme is suitable for all HLS-enabled players.

#### 1. Log in and distribute the `Token` used for authentication
Only authenticated end users can get a video decryption key. Therefore, before a video is played back, the client must be logged in to, and then the application server will distribute to it a signature containing the authentication information, which is called a `Token`.

#### 2. Get the multi-bitrate playback address containing key hotlink protection signature
You can get the multi-bitrate playback address of an encrypted video from either the callback notification of the ProcessFile API or the GetVideoInfo API.
After the multi-bitrate playback address is obtained, the client needs to add the end user's identity information to the address. To do so for any playback URL, add `voddrm.token.<Token>` before the **filename** in the URL.

For example, if the end user's ID is ABC123, and the playback URL for a certain bitrate is:
<pre>
http://example.vod2.myqcloud.com/path/to/a/video.m3u8
</pre>

Then the final URL will be:
<pre>
http://example.vod2.myqcloud.com/path/to/a/voddrm.token.ABC123.video.m3u8
</pre>


#### 3. Get video content (encrypted)

When a player accesses the URL that carries the end user's identity information as described in the previous step, the VOD backend will automatically append the `Token` information as a `QueryString` to the URL identified by the `EXT-X-KEY` tag of the source M3U8 file.

For example, if the encrypted video URL for a certain bitrate is:
<pre>
http://example.vod2.myqcloud.com/path/to/a/video.m3u8
</pre>

In this file, the URL for obtaining the video decryption key identified by the `EXT-X-KEY` tag is:
<pre>
https://getkey.example.com?fileId=123456&keySource=VodBuildInKMS&edk=abcdef
</pre>

When the player accesses the playback URL carrying the `Token` information, i.e.,
<pre>
http://example.vod2.myqcloud.com/path/to/a/voddrm.token.ABC123.video.m3u8
</pre>

The URL for obtaining the video decryption key identified by the `EXT-X-KEY` tag will be replaced with:
<pre>
https://getkey.example.com?fileId=123456&keySource=VodBuildInKMS&edk=abcdef&token=ABC123
</pre>

At this time, when the player tries to get the decryption DK, it will bring the `Token` distributed in step 1.

#### 4. Get video decryption key (with authentication cookie)

After the player gets the video index file (M3U8 file), it will automatically initiate step 4 before playing back the video file. After the application backend receives the client's request, it will first verify the `Token` in the `QueryString`; if the end user's identity is invalid, it will directly reject the request; and if the end user's identity is valid, it will get the DK from the KMS system based on parameters such as `fileId`, `keySource` and `edk` in the URL and return it to the client.
After the above steps are completed, the client can get the video decryption key and perform video decryption and playback.

### <span id="p2"></span>Video playback scheme 2
Video playback scheme 2: passing authentication information through cookie. This scheme is only suitable for HTML5/Flash players on iOS devices or PCs. In this case, the player will bring a cookie when accessing the URL identified by the `EXT-X-KEY` tag.

> Tests showed that the HTML5 player on Android did not carry cookies when accessing the URL identified by the `EXT-X-KEY` tag, so for the Android platform, only **scheme 1** can be used currently.

#### 1. Log in and distribute the cookie used for authentication
Only authenticated end users can get a video decryption key. Therefore, before a video is played back, the client must be logged in to, and then the application server will distribute to it a signature. For example, after the client is logged in to with an account and password at `login.example.com` and authentication is passed on the application backend, the cookie of the `example.com` domain will be distributed to the client to identify the end user.

#### 2. Get the multi-bitrate playback address of a specified video
The web player of VOD supports multi-bitrate playback, which can get a multi-bitrate playback address of a video based on the `fileId`. If you use other players, you must get the multi-bitrate playback address on your own.

#### 3. Get video content (encrypted)
The player will automatically perform this step when it starts playing back the video.
When the player starts playing back the video, it will request the video data file from a CDN edge server of VOD. For videos in HLS format, the player will get the video decryption key based on the `EXT-X-KEY` tag in the M3U8 file. For example, if the URL to get the video decryption key in the `EXT-X-KEY` tag is:
<pre>
https://getkey.example.com?fileId=123456&keySource=VodBuildInKMS&edk=abcdef
</pre>

Then, when the player gets the decryption DK, it will bring the cookie of the `example.com` domain distributed by the application backend in step 1.

#### 4. Get video decryption key (with authentication cookie)

After the player gets the video index file (M3U8 file), it will automatically initiate step 4 before playing back the video file.

After the application backend receives the client's request, it will first verify the identifier in the cookie; if the end user's identity is invalid, it will directly reject the request; and if the end user's identity is valid, it will get the DK from the KMS system based on parameters such as `fileId`, `keySource` and `edk` in the URL and return it to the client.
After the above steps are completed, the client can get the video decryption key and perform video decryption and playback.

## FAQs

#### 1. What are the differences between encrypted HLS and common HLS?
According to the HLS specifications, HLS encryption is to encrypt media files (TS files), and their M3U8 files describe how the player decrypts them. The M3U8 files of encrypted HLS contain an `EXT-X-KEY` tag with the `METHOD` and `URI` attributes. `METHOD` describes the encryption algorithm such as `AES-128`, while `URI` describes the address for obtaining the decryption key, which can be accessed by the player to get the key data used for decryption. If `URI` is:
<pre>
http://www.test.com/getdk?fileId=123&edk=14cf
</pre>

Then, when the player parses the M3U8 file, it will make an HTTP request to this URI and get the key data from the return packet.

#### 2. What information is required to enable VOD encryption?
To enable VOD encryption, you need to provide `getkeyurl`, i.e., the `URI` attribute in the `EXT-X-KEY` tag.
The application server needs to deploy an HTTP service that gets key data when the client plays back an encrypted video. When the VOD service encrypts the video, the `URI` attribute of the `EXT-X-KEY` tag in the encrypted video's M3U8 file will be set to `getkeyurl`. In order to get the key and manage it conveniently, three parameters (`fileId`, `edk` and `keySource`) will be appended to `getkeyurl`.
> `fileId` is the video ID, `edk` is the encrypted key, and `keySource` is the key source. For files encrypted with the KMS system built in VOD, `keySource` is `VodBuildInKMS`. When the player initiates a request to get the decryption key, the `QueryString` of the HTTP request received by the application server will include parameters such as `fileId` and `edk`, based on which the application server can return the corresponding DK to the player.

#### 3. Where does the player get the decryption key when playing back an encrypted video?
When the player plays back an encrypted video, it initiates a request for obtaining the key based on the URI of the `EXT-X-KEY` tag in the M3U8 file, which is the `getkeyurl` address provided by the application to VOD.
<!--api > The player does not make a request to the VOD server for obtaining the key. When the application server calls the ProcessFile API of VOD for encryption, it will call the [DescribeDrmDataKey](https://intl.cloud.tencent.com/document/product/266/9643) API to get the key after encryption is completed.--> After that, the key needs to be stored, and when the player requests the corresponding key, it will be returned according to the player's request parameters.

#### 4. How does the application server return the key to the player?
When the player requests the key from the application server, the application server needs to return binary key data of 16 bytes to the player. <!--api However, the key obtained through the [DescribeDrmDataKey](https://intl.cloud.tencent.com/document/product/266/9643) API is a Base64-encoded string. Therefore, the string should be converted to binary data before it is returned to the player.-->
For example, dkData is Base64-encoded key data:

Java
```java
  import java.util.Base64;
  byte[] dkBin = Base64.getDecoder().decode(dkData);
```  

PHP
```php
$dkBin = base64_decode($dkData);
```
