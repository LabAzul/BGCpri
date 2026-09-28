<a id="readme-top"></a>  
# BGCpri
BGCpri is an algorithm designed to prioritize biosynthetic gene clusters (BGCs) of polyketide synthases (PKS), non-ribosomal peptide synthetases (NRPS), and hybrid NRPS/PKS identified through antiSMASH results. It employs a scoring system tailored to prioritize BGCs with the potential to encode chemically diverse secondary metabolites.

## Table of Contents
- [Getting Started](#getting-started)  
- [Installing BGCpri on Linux](#installing-bgcpri-on-linux)  
- [Using BGCpri](#using-bgcpri)  
- [BGCpri Results](#bgcpri-results)  
- [Testing BGCpri](#testing-bgcpri)  
- [Contributing](#contributing)  
- [License](#license)  
- [Credits](#credits)

## Getting Started
### Requirements
- A Linux terminal, on any Linux distribution, or on Windows via [WSL](https://learn.microsoft.com/en-us/windows/wsl/install);
- [antiSMASH](https://antismash.secondarymetabolites.org/) results (version 6.0 or later) for the genome(s) or metagenome(s) of interest, as individual `.gbk` files or as a single `.zip` archive;
- The `unzip` utility;
- The `bc` command-line calculator.
BGCpri requires the `unzip` and `bc` dependencies to be installed:

### **Installing `unzip` and `bc`**  
`unzip` is necessary for BGCpri to extract antiSMASH compressed .zip files, and `bc` is necessary for BGCpri to compute prioritization scores. If either is missing, BGCpri will report this explicitly on startup and exit without attempting to run.
#### For Debian-based systems (e.g., Ubuntu or Linux Mint) or Windows via Ubuntu WSL:
```  
sudo apt update && sudo apt install unzip bc
```
#### For Redhat distributions (e.g., Fedora or CentOS):
```  
sudo dnf install unzip bc
```
#### For Arch-based distributions (e.g., Arch Linux and Manjaro):
```  
sudo pacman -S unzip bc
```
#### For OpenSUSE:
```  
sudo zypper install unzip bc
```

## **Installing BGCpri on Linux**  
To install BGCpri on a Linux system, follow these steps:
### 1. Download BGCpri
First, download the BGCpri application from the [GitHub repository](https://github.com/labAzul/BGCpri) .
### 2. Extract BGCpri
Extract the downloaded BGCpri archive to a directory of your choice. You can do this by using the `unzip` command in the terminal.
```  
unzip BGCpri.zip -d /path/to/extract
```
Replace `/path/to/extract` with the directory where you want to extract BGCpri.
### 3. Navigate to the BGCpri Directory
Open a terminal and navigate to the directory where you extracted BGCpri.
```  
cd /path/to/extract/BGCpri
```
### 4. Set Permissions (if necessary)
Depending on your system's configuration, you may need to set executable permissions for the BGCpri executable file.
```  
chmod +x BGCpri
```
### 5. Add BGCpri to PATH (highly recommended)
For convenience, you may want to add the directory containing the BGCpri executable to your system's PATH. This step is optional but can make it easier to run BGCpri from any directory in the terminal.
```  
export PATH="/path/to/extract/BGCpri:$PATH"
```
Replace `/path/to/extract/BGCpri` with the actual path to the directory containing the BGCpri executable. You can add this line to your shell configuration file (e.g., `~/.bashrc`, `~/.bash_profile`, `~/.zshrc`, etc.) to make the change permanent.
### 6. Verify Installation
To verify that BGCpri is installed correctly, open a new terminal window and type:
```  
BGCpri --version
```
If installed properly, this command should display the version of BGCpri installed on your system.
Following these steps will enable you to successfully install BGCpri on your system.
<p align="right">(<a href="#readme-top">back to top</a>)</p>

## Using BGCpri  
Once you have installed the dependencies and BGCpri itself, you can use it to prioritize Biosynthetic Gene Clusters based on antiSMASH results. Here's how you can get started:
### 1. **Prepare antiSMASH Results**  
Ensure that you have run antiSMASH (version 6.0 or later) on your genome or metagenome of interest and obtained the result files in `.zip` or `.gbk` (for individual BGCs) format. BGCpri accepts either individual `.gbk` files or a single `.zip` archive containing them, placed in the same input directory.
**Important:** BGCpri only processes `.gbk` files whose name contains the word `region` (lowercase), matching antiSMASH's default per-cluster output naming convention (e.g. `contig1.region001.gbk`). If you rename your `.gbk` files, make sure `region` (lowercase) still appears in each file name.
Also, check if the .gbk files contain the name of the strain in the ‘ORGANISM’ section, like the following example:
```  
LOCUS       QZCE01000001           21245 bp    DNA     linear   BCT 25-FEB-2020  
DEFINITION  Adonisia turfae CCMR0082 Scaffold_1a, whole genome shotgun sequence.  
ACCESSION   QZCE01000001  
VERSION     QZCE01000001.1  
KEYWORDS    .  
SOURCE      Adonisia turfae CCMR0082  
ORGANISM  Adonisia turfae CCMR0082  
```
If it’s empty, run the following command to add the organism’s name on multiple .gbk files inside the same directory:
```  
sed -i 's|  ORGANISM|  ORGANISM  StrainName|g' *.gbk  
```
Replace ‘StrainName’ with the actual name of the strain.
### 2. **Execute BGCpri**  
If you have installed BGCpri in your system, then run the following command:
```  
BGCpri -i /path/to/antiSMASH/results -o /path/to/BGCpri/output/results
```  
Replace ‘/path/to/antiSMASH/results’ with the actual path to the directory containing your antiSMASH results of interest and replace /path/to/BGCpri/output/results with the path to the desired output destination for the output results to be saved.
If you haven't installed BGCpri in your system, then navigate to the directory where the BGCpri executable is located and run the following command:
```  
./BGCpri -i /path/to/antiSMASH/results -o /path/to/BGCpri/output/results
```  
Replace ‘/path/to/antiSMASH/results’ with the actual path to the directory containing your antiSMASH results of interest and replace /path/to/BGCpri/output/results with the path to the desired output destination for the output results to be saved.
<p align="right">(<a href="#readme-top">back to top</a>)</p>

## BGCpri results
After the execution, BGCpri will create a directory called “BGC_PRIORITIZATION_RESULTS”. Inside this directory you will find the following file:
**BGC_summary.tsv:** the table of prioritization results, with one row per antiSMASH-annotated region.
<p align="right">(<a href="#readme-top">back to top</a>)</p>

## Testing BGCpri
BGCpri comes with a test suite in the `tests/` directory. It runs BGCpri on a small set of example antiSMASH region files, some valid and some deliberately malformed, and checks that each one is either scored correctly or skipped with a warning. We recommend running it after installing BGCpri, or after any change to the script.
### Running the tests
From the BGCpri directory, run:
```  
./tests/run_test
```
To test a BGCpri executable stored somewhere else, give its path as an argument:
```  
./tests/run_test /path/to/BGCpri
```
If any check fails, the test script prints `FAIL` next to that check, shows the relevant BGCpri output, and exits with a non-zero status.

## Contributing
Contributions to BGCpri are welcome! If you encounter any issues, have suggestions for improvements, or would like to contribute new features, please feel free to open an issue or submit a pull request on the [GitHub repository](https://github.com/LabAzul/BGCpri).
<br>

## License
BGCpri is licensed under the [GPLLicense](https://www.gnu.org/licenses/gpl-3.0.html). See the [LICENSE](LICENSE) file for details.
<br>

## Credits  
BGCpri was developed by João Pedro Brandão Domingues, Lígia Akemi Mizuyama, Laura Pavan Ióca, Alessandra S. Eustáquio, and Camila Manoel Crnkovic.
