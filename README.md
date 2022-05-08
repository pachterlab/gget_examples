# gget_examples

##gget ref
### Use in combination with [kallisto | bustools](https://www.kallistobus.tools/kb_usage/kb_ref/) to build a reference

```bash
kb ref -i INDEX -g T2G -f1 FASTA $(gget ref --ftp -w dna,gtf -s homo_sapiens)
```
&rarr; kb ref builds a reference using the latest DNA and GTF files of species **Homo sapiens** passed to it by gget ref

#### Show all available species

```python
# Jupyter Lab / Google Colab:
!gget ref --list

# Terminal:
$ gget ref --list
```
&rarr; Returns a list with all available species from the latest Ensembl release.

#### Fetch GTF, DNA, and cDNA FTP links for a specific species

```python
# Jupyter Lab / Google Colab:
ref("homo_sapiens")

# Terminal:
$ gget ref -s homo_sapiens
```
&rarr; Returns a json with the latest links to human GTF and FASTA files, their respective release dates and time, and the Ensembl release from which the links were fetched, in the format:
```
{
            species: {
                "transcriptome_cdna": {
                    "ftp": cDNA FTP download URL,
                    "ensembl_release": Ensembl release,
                    "release_date": Day-Month-Year,
                    "release_time": HH:MM,
                    "bytes": cDNA FTP file size in bytes
                },
                "genome_dna": {
                    "ftp": DNA FTP download URL,
                    "ensembl_release": Ensembl release,
                    "release_date": Day-Month-Year,
                    "release_time": HH:MM,
                    "bytes": DNA FTP file size in bytes
                },
                "annotation_gtf": {
                    "ftp": GTF FTP download URL,
                    "ensembl_release": Ensembl release,
                    "release_date": Day-Month-Year,
                    "release_time": HH:MM,
                    "bytes": GTF FTP file size in bytes
                }
            }
        }
```

#### Fetch GTF, DNA, and cDNA FTP links for a specific species from a specific Ensembl release
For example, for Ensembl release 104:  


```python
# Jupyter Lab / Google Colab:
ref("homo_sapiens", release=104)

# Terminal
$ gget ref -s homo_sapiens -r 104
```
&rarr; Returns a json with the human reference genome GTF, DNA, and cDNA links, and their respective release dates and time, from Ensembl release 104.

#### Save the results

```python
# Jupyter Lab / Google Colab:
ref("homo_sapiens", save=True)

# Terminal 
$ gget ref -s homo_sapiens -o path/to/directory/ref_results.json
```
&rarr; Saves the results in path/to/directory/ref_results.json.  
For Jupyter Lab / Google Colab: Saves the results in a json file named ref_results.json in the current working directory.  
  
Note: To download the files linked to by the FTPs into the current directory, add flag `-d`.

#### Fetch only certain types of links for a specific species 

```python
# Jupyter Lab / Google Colab:
ref("homo_sapiens", which=["gtf", "dna"])


# Terminal 
$ gget ref -s homo_sapiens -w gtf,dna
```
&rarr; Returns a dictionary/json containing the latest human reference GTF and DNA files, in this order, and their respective release dates and time.    

#### Fetch only certain types of links for a specific species and return only the links

```python
# Jupyter Lab / Google Colab:
ref("homo_sapiens", which=["gtf", "dna"], ftp=True)

# Terminal 
$ gget ref -s homo_sapiens -w gtf,dna -ftp
```
&rarr; Returns only the links (wihtout additional information) to the latest human reference GTF and DNA files, in this order, in a space-separated list (terminal), or comma-separated list (Jupyter Lab / Google Colab).    
For Jupyter Lab / Google Colab: Combining this command with `save=True`, will save the results in a text file named ref_results.txt in the current working directory.

___
##gget search

#### Query Ensembl for genes from a specific species using multiple searchwords

```python
# Jupyter Lab / Google Colab:
search(["gaba", "gamma-aminobutyric"], "homo_sapiens")

# Terminal 
$ gget search -sw gaba,gamma-aminobutyric -s homo_sapiens
```
&rarr; Returns all genes that contain at least one of the searchwords in their Ensembl or external reference description, in the format:

| Ensembl_ID     | Ensembl_description     | Ext_ref_description     | Biotype        | Gene_name | URL |
| -------------- |-------------------------| ------------------------| -------------- | ----------|-----|
| ENSG00000034713| GABA type A receptor associated protein like 2 [Source:HGNC Symbol;Acc:HGNC:13291] | 	GABA type A receptor associated protein like 2 | protein_coding | GABARAPL2 | https://uswest.ensembl.org/homo_sapiens/Gene/Summary?g=ENSG00000034713 |
| . . .            | . . .                     | . . .                     | . . .            | . . .       | . . . |


#### Query Ensembl for transcripts from a specific species which include ALL searchwords
```python
# Jupyter Lab / Google Colab:
search(["gaba", "gamma-aminobutyric"], "nothobranchius_furzeri", d_type="transcript", andor="and")

# Terminal 
$ gget search -sw gaba,gamma-aminobutyric -s nothobranchius_furzeri -t transcript -ao and
```
&rarr; Returns all killifish transcripts that contain all of the searchwords in their Ensembl or external reference description.


#### Query Ensembl for genes from a specific species using a single searchword and while limiting the number of returned search results 

```python
# Jupyter Lab / Google Colab:
search("gaba", "homo_sapiens", limit=10)

# Terminal 
$ gget search -sw gaba -s homo_sapiens -l 10
```
&rarr; Returns the first 10 genes that contain the searchword in their Ensembl or external reference description. If more than one searchword is passed, `limit` will limit the number of genes per searchword. 

#### Query Ensembl for genes from any of the 236 species databases found [here](http://ftp.ensembl.org/pub/release-105/mysql/), e.g. a specific mouse strain.   

```python
# Jupyter Lab / Google Colab:
search("brain", "mus_musculus_cbaj_core_105_1")

# Terminal 
$ gget search -sw brain -s mus_musculus_cbaj_core_105_1 
```
&rarr; Returns genes from the CBA/J mouse strain that contain the searchword in their Ensembl or external reference description.

___
##gget info
#### Look up a list of gene Ensembl IDs including information on all isoforms

```python
# Jupyter Lab / Google Colab:
info(["ENSG00000034713", "ENSG00000104853", "ENSG00000170296"], expand=True)

# Terminal 
$ gget info -id ENSG00000034713,ENSG00000104853,ENSG00000170296 -e
```
&rarr; Returns a json containing information about each ID, amongst others the common name, description, and corresponding transcript/gene, in the format:
```
{
            "Ensembl ID": {
                        "species": genus_species,
                        "object_type": e.g. Gene,
                        "biotype": Gene biotype, e.g. protein_coding,
                        "display_name": Common gene name,
                        "description": Ensemble description,
                        "assembly_name": Name of species assmebly,
                        "seq_region_name": Sequence region,
                        "start": Sequence start position,
                        "end": Sequence end position,
                        "strand": Strand
                        "canonical_transcript": Transcript ID,
                        # All transcript isoforms:
                        "Transcript": [{'display_name': Transcript name,
					'biotype': Transcript biotype,
					'id': Transcript ID}, ...]
                        },
}
```

Note: When looking up Ensembl IDs of transcripts instead of genes, the "Transcript" entry above will be replaced by "Translation" and "Exon" information. 

#### Look up a transcript Ensembl ID and include external reference descriptions

```python
# Jupyter Lab / Google Colab:
info("ENSDART00000135343", xref=True)

# Terminal 
$ gget info -id ENSDART00000135343 -x
```
&rarr; Returns a json containing the homology information, and external reference description of each ID in addition to the standard information mentioned above.
___

##gget seq

#### Fetch the sequences of several transcript Ensembl IDs
```python
# Jupyter Lab / Google Colab:
seq(["ENST00000441207","ENST00000587537"])

# Terminal 
$ gget seq -id ENST00000441207,ENST00000587537
```
&rarr; Returns a FASTA containing the sequence of each ID, in the format:
```
>Ensembl_ID chromosome:assembly:seq_region_name:seq_region_start:seq_region_end:strand
GGGAATGGAAATCTGTCCCTCGTGCTGGAAGCCAACCAGTGGTGATGACTCTGTGTGCCACTCCGCCTCCTACAGCGCGGATCCTCTG  
CGTGTGTCCTCGCAAGACAAGCTCGATGAAATGGCCGAGTCCAGTCAAGCAAACTTTGAGGGAA...
```

#### Fetch the sequences of a gene Ensembl ID and all its transcript isoforms
```python
# Jupyter Lab / Google Colab:
seq("ENSMUSG00000025040", isoforms=True)

# Terminal 
$ gget seq -id ENSMUSG00000025040 -i
```
&rarr; Returns a FASTA containing the sequence of the gene ID and the sequences of all of each transcripts.
