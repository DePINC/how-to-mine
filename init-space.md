# Initialize Space

DePINC supports both standard Chia-generated plots and third-party compressed plots. For instructions on using Chia's official disk initialization program, visit [Chia's farming guide](https://docs.chia.net/farming-guide/). Here we'll focus on initializing disks with Bladebit, a third-party tool for creating compressed plots with C20 support.



### Download Bladebit

For optimal performance, download the compiled Bladebit tool with C20 compressed plot support from the following link:

[Bladebit C20 Compressed Plot](https://github.com/forthemoonlight/Bladebit-depinc/releases/tag/v0.0.1)

Note: Bladebit is currently only available for Linux. For optimal performance when creating C20 compressed plots, it is recommended to use a Linux system. If you do not have access to a Linux machine, consider using a virtual machine or cloud computing service to run Bladebit on Linux.

### Using Bladebit

To use Bladebit, replace `<farmer_pub_key>` with your actual farmer public-key from your Chia wallet in the command below:

```bash
Bladebit -z 20 -f <farmer_pub_key> -t1 /my/temporary/plot/dir -n 100 cudaplot /my/output/dir
```

Parameter explanation:

1. '-z 20': Compression ratio. Higher values produce smaller files but increase CPU usage during mining.
2. '-f <farmer_pub_key>': Your Farmer public key from the Chia wallet. Replace with your actual key (omit brackets).
3. '-t1 /my/temporary/plot/dir': Temporary file storage location. Modify this path; using an SSD is recommended.
4. '-n 100': Number of files to generate. Adjust as needed; Bladebit exits if disk space is insufficient.
5. 'cudaplot': Utilizes GPU for disk initialization. Requires an NVIDIA GPU with CUDA support.
6. '/my/output/dir': Final plot storage location. Modify to point to your desired hard drive directory.

Please note:

- Use an SSD for temporary storage if possible
- Close other resource-intensive programs
- Ensure adequate free disk space
- Consider running overnight for large plot counts
- Monitor system temperatures to prevent throttling

The process may take several hours for a large number of plots. You can check progress in the Bladebit output.
