# Set up a Galaxy instance with default configurations
- see: https://galaxyproject.org/admin/get-galaxy/
## Basic requirements
1. UNIX/Linux or Mac OSX
2. Python 3.7 or newer. 
	- `pip install virtualenv`
## Set up with default configurations
- Decide if this instance will be used for a single user or Production 
- Get a clone of latest Galaxy release: `git clone -b release_22.05 https://github.com/galaxyproject/galaxy.git`
- After fetching the Galaxy repo, go to the directory and start server using `bash run.sh`
- If you need any python package required install it.
- It may run the galaxy server on `http://localhost:8080`
- To stop the Galaxy server, use Ctrl-C in the terminal window from which Galaxy is running.

## Configure
- you can configure the configurations by editing: `config/galaxy.yml` file.
	- in uWSGI section change the server IP PORT edit the line with `http: 127.0.0.1:8080` to `http: 192.168.101.7:8080`
	- By default uWSGI allocates a very small buffer (4096 bytes) for the headers of each request. If you start receiving "invalid request block size" in your logs, it could mean you need a bigger buffer. 
	- recommended buffer is at least 16384. Change the line : `buffer-size: 16384`

## Production level set-up
- see: `https://docs.galaxyproject.org/en/latest/admin/production.html`
- By default, Galaxy will use SQLite, which is a serverless simple file database engine.
- Postgres database backend is far more used and better tested


## Installing a new Tool into Galaxy
- See: `https://galaxyproject.org/admin/tools/add-tool-from-toolshed-tutorial/`
- Install a tool from the Tool Shed.
- Custome tools can be added to your Galaxy instance manually.

- for Custom tools: `https://galaxyproject.org/admin/tools/add-tool-tutorial/`
- Make a folder in the tools/ directory `private_tools`
- Put the tool binary file/scripts (e.g myTool.pl) to `tools/private_directory` and add correspongs tool defination XML file named `myTool.xml` with following codes:

```xml
<tool id="fa_gc_content_1" name="Compute GC content" version="0.1.0">
  <description>for each sequence in a file</description>
  <command>perl '${__tool_directory__}/toolExample.pl' '$input' '$output'</command>
  <inputs>
    <param format="fasta" name="input" type="data" label="Source file"/>
  </inputs>
  <outputs>
    <data format="tabular" name="output" />
  </outputs>

  <tests>
    <test>
      <param name="input" value="fa_gc_content_input.fa"/>
      <output name="out_file1" file="fa_gc_content_output.txt"/>
    </test>
  </tests>

  <help>
This tool computes GC content from a FASTA file.
  </help>

</tool>
```
- Edit the `config/tools_config.yml` and add the following code

```yml
 <section name="MyTools" id="mTools">
    <tool file="private_tools/toolExample1.xml" />
    <tool file="private_tools/plot_venn.xml" />
    <tool file="private_tools/muscle.xml" />
    <tool file="private_tools/fasttree.xml" />
    <tool file="private_tools/gotree_draw.xml" />
 </section>
```

