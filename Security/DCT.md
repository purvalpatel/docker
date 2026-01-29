DCT ( Docker Content Trust )
----------------------------
- Docker content trust (DCT) provides the ability to use **digital signatures** for data send to and received from remote docker registries.
- hese signatures allow client-side or runtime verification of the integrity and publisher of specific image tags.
<br>

**Through DCT, image publishers can sign their images and image consumers can ensure that the images they pull are signed.** <br>
**Verify that Docker image is not tempared.** <br>
<br>
**It ensures:**
- The image really comes from who you expect
- The image was not changed after it was built

Generate key:
```
docker trust key generate jeff
```
Or Use existing key:
```
docker trust key load key.pem --name jeff
```
### Enable it:

Only pull signed image. <br>
With DCT.
```
export DOCKER_CONTENT_TRUST=1
docker pull nginx:latest
```

### Disable it:
Without DCT
```
docker pull nginx:latest
export DOCKER_CONTENT_TRUST=0
```

Generate key:
```
docker trust key generate jeff
```
Add this new created sign to image:
```
docker trust signer add --key cert.pem jeff registry.example.com/admin/demo
```
Sign the trust data to image tag:
```
docker trust sign registry.example.com/admin/demo:1
```
Remove the trust from the image:
```
docker trust revoke registry.example.com/admin/demo:1
```
