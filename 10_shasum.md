# 10. shasum - Print or Check SHA Checksums 

A checksum is a sequence of numbers and letters used to check data for errors. <br>
If you know the checksum of an original file, you can use a checksum utility to confirm your copy is identical.

1. Make sure that you are in the right actual state
2. Open a terminal
3. `$ cd sgoinfre/Perso/mchampag/VMs/Born2beRoot/Born2beRoot`
4. `$ shasum Born2beRoot.vdi > signature.txt`
5. ⏳ wait ⏳ the more the VM is big, the more it takes time...
6. `cat signature.txt`
    - example : ff9e0bf30d5e45eb4b9be9b4fdf8b31a657500cd  Born2beRoot.vdi

# Documentation

- [What is checksum](https://www.howtogeek.com/363735/what-is-a-checksum-and-why-should-you-care/)
