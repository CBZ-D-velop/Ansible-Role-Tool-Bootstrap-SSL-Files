# Ansible role: tool.bootstrap_ssl_files

![Licence Status](https://img.shields.io/badge/licence-MIT-brightgreen)
![CI Status](https://img.shields.io/badge/CI-success-brightgreen)
![Testing Method](https://img.shields.io/badge/Testing%20Method-Ansible%20Molecule-blueviolet)
![Testing Driver](https://img.shields.io/badge/Testing%20Driver-docker-blueviolet)
![Language Status](https://img.shields.io/badge/language-Ansible-red)
![Compagny](https://img.shields.io/badge/Compagny-Labo--CBZ-blue)
![Author](https://img.shields.io/badge/Author-Lord%20Robin%20Cbz-blue)

## Description

![Tag: Ansible](https://img.shields.io/badge/Tech-Ansible-orange)
![Tag: Debian](https://img.shields.io/badge/Tech-Debian-orange)
![Tag: OpenSSL](https://img.shields.io/badge/Tech-OpenSSL-orange)
![Tag: SSL/TLS](https://img.shields.io/badge/Tech-SSL%2FTLS-orange)
![Tag: KEY](https://img.shields.io/badge/Tech-KEY-orange)
![Tag: REQ](https://img.shields.io/badge/Tech-REQ-orange)
![Tag: CSR](https://img.shields.io/badge/Tech-CSR-orange)
![Tag: CRT](https://img.shields.io/badge/Tech-CRT-orange)
![Tag: PEM](https://img.shields.io/badge/Tech-PEM-orange)
![Tag: CA](https://img.shields.io/badge/Tech-CA-orange)

An Ansible role create a list of defined files related to SSL/TLS

The Certificate Management role automates the creation and organization of certificates, including Certificate Authorities (CAs), root CAs, intermediate CAs, and end certificates. This role simplifies the process of generating certificates and provides a streamlined approach for managing SSL/TLS infrastructure. Key features of this role include:

Certificate Creation: The role generates various types of certificates, including root CAs, intermediate CAs, and end certificates. It allows you to define the desired SSL/TLS information, such as Common Name (CN), Country (C), State (ST), Locality (L), Organization (O), Organizational Unit (OU), and email address.

Certificate Filing: The role organizes the generated certificates into separate components and creates a ZIP archive for each component. This makes it easy to deploy and distribute the certificates to the desired target systems.

Customization Options: The role provides flexibility by allowing you to specify the validity period, key size, and base path for storing the certificate files. You can customize these parameters to meet your specific requirements.

By utilizing the Certificate Management role, you can simplify the creation and management of SSL/TLS certificates, ensure proper organization and filing of certificates, and streamline the deployment process.

## Folder structure

By default Ansible will look in each directory within a role for a main.yml file for relevant content (also man.yml and main):

```PYTHON
.
├── README.md  # Contains an overview of the role and its purpose.
├── defaults
│   ├── main.yml  # Contains default variables for the role that can be overridden by users.
│   └── README.md  # Contains documentation for the default variables.
├── files
│   └── README.md  # Contains documentation for the files in the directory.
├── handlers
│   ├── main.yml  # Contains handlers that can be called by tasks within the role.
│   └── README.md  # Contains documentation for the handlers.
├── meta
│   ├── main.yml  # Contains metadata about the role, including dependencies and supported platforms.
│   └── README.md  # Contains documentation for the role metadata.
├── tasks
│   ├── main.yml  # Contains tasks to be executed by the role on the managed nodes.
│   └── README.md  # Contains documentation for the tasks.
├── templates
│   └── README.md  # Contains documentation for the templates.
└── vars
    ├── main.yml  # Contains variables that are specific to the role and are not meant to be overridden.
    └── README.md  # Contains documentation for the role variables.
```

## Tests and simulations

### Basics

You have to run multiples tests. *tests with an # are mandatory*

```MARKDOWN
# lint
# syntax
# converge
# idempotence
# verify
side_effect
```

Executing theses test in this order is called a "scenario" and Molecule can handle them.

Molecule use Ansible and pre configured playbook to create containers, prepare them, converge (run the role) and verify its execution.
You can manage multiples scenario with multiples tests in order to get a 100% code coverage.

This role contains a ./tests folder. In this folder you can use the inventory or the tower folder to create a simualtion of a real inventory and a real AWX / Tower job execution.

### Command reminder

```SHELL
# Check your YAML syntax
yamllint -c ./.yamllint .

# Check your Ansible syntax and code security
ansible-lint --config=./.ansible-lint .

# Execute and test your role
molecule lint
molecule create
molecule list
molecule converge
molecule verify
molecule destroy

# Execute all previous task in one single command
molecule test
```

## Installation

To install this role, just copy/import this role or raw file into your fresh playbook repository or call it with the "include_role/import_role" module.

## Usage

### Vars

Some vars a required to run this role:

```YAML
---
# The current user and group to create files
bootstrap_ssl_files__user: "root"
# The base path on where you want your certs files
bootstrap_ssl_files__base_path: "/root/ssl"
# The validity of your certs files
bootstrap_ssl_files__ca_validity: 3650
bootstrap_ssl_files__cert_validity: 365
# The size of your key
bootstrap_ssl_files__key_size: 4096

bootstrap_ssl_files__serial_start: 1000
bootstrap_ssl_files__dirs:
  - "certs"
  - "crl"
  - "newcerts"
  - "private"
  - "csr"
  - "req"
  - "bundles"

# SSL/TLS informations
inv_bootstrap_ssl_files__root_ca:
  cn: "My Local Ansible Root CA"
  c: "FR"
  st: "state"
  l: "city"
  o: "Local Ansible"
  ou: "My Local Ansible Root CA"
  email_address: "contact@your.domain.tld"
  password: "m3EH3A56h5mNY"
  ca_password: "m3EH3A56h5mNY"

inv_bootstrap_ssl_files__intermediates_ca:
  - cn: "My Local Ansible Intermediate CA 1"
    c: "FR"
    st: "state"
    l: "city"
    o: "Local Ansible"
    ou: "My Local Ansible Intermediate CA 1"
    email_address: "contact@your.domain.tld"
    ca_password: "m3EH3A56h5mNY"
    password: "jdieldodkiekie"
    certification_ca: "My Local Ansible Root CA"

  - cn: "My Local Ansible Intermediate CA 2"
    c: "FR"
    st: "state"
    l: "city"
    o: "Local Ansible"
    ou: "My Local Ansible Intermediate CA 2"
    email_address: "contact@your.domain.tld"
    ca_password: "jdieldodkiekie"
    password: "kdidkodkdokldkdokeok"
    certification_ca: "My Local Ansible Intermediate CA 1"

bootstrap_ssl_files__end_certs:
  - cn: "my-end-certificate-1.domain.tld"
    c: "FR"
    st: "state"
    l: "city"
    o: "Local Ansible"
    ou: "Local Dev IT"
    email_address: "contact@your.domain.tld"
    alternatives:
      - "127.0.0.1"
      - "localhost"
      - "my-website.tld"
    certification_ca: "My Local Ansible Intermediate CA 1"

  - cn: "my-end-certificate-2.domain.tld"
    c: "FR"
    st: "state"
    l: "city"
    o: "My Local Intermediate CA Two"
    ou: "Local Dev IT"
    email_address: "contact@your.domain.tld"
    alternatives:
      - "127.0.0.1"
      - "localhost"
      - "my-website.tld"
    certification_ca: "My Local Ansible Intermediate CA 2"

  - cn: "my-end-certificate-3.domain.tld"
    c: "FR"
    st: "state"
    l: "city"
    o: "Local Ansible"
    ou: "Local Dev IT"
    email_address: "contact@your.domain.tld"
    alternatives:
      - "127.0.0.1"
      - "localhost"
      - "my-website.tld"
    certification_ca: "My Local Ansible Intermediate CA 2"

```

The best way is to modify these vars by copy the ./default/main.yml file into the ./vars and edit with your personnals requirements.

You can set vars in the template model in Ansible AWX / Tower or just surchage them during the playbook call.

In order to surchage vars, you have multiples possibilities but for mains cases you have to put vars in your inventory and/or on your AWX / Tower interface.

```YAML
---
# The current user and group to create files
inv_bootstrap_ssl_files__user: "root"
# The base path on where you want your certs files
inv_bootstrap_ssl_files__base_path: "/tmp/ssl/MyPKI"
# The validity of your certs files
inv_bootstrap_ssl_files__ca_validity: 3650
inv_bootstrap_ssl_files__cert_validity: 90
# The size of your key
inv_bootstrap_ssl_files__key_size: 4096
# Files wanted and where you want them

# SSL/TLS informations
inv_bootstrap_ssl_files__root_ca:
  cn: "My Local Ansible Root CA"
  c: "FR"
  st: "state"
  l: "city"
  o: "Local Ansible"
  ou: "My Local Ansible Root CA"
  email_address: "contact@your.domain.tld"
  password: "m3EH3A56h5mNY"
  ca_password: "m3EH3A56h5mNY"

inv_bootstrap_ssl_files__intermediates_ca:
  - cn: "My Local Ansible Intermediate CA 1"
    c: "FR"
    st: "state"
    l: "city"
    o: "Local Ansible"
    ou: "My Local Ansible Intermediate CA 1"
    email_address: "contact@your.domain.tld"
    ca_password: "m3EH3A56h5mNY"
    password: "jdieldodkiekie"
    certification_ca: "My Local Ansible Root CA"

  - cn: "My Local Ansible Intermediate CA 2"
    c: "FR"
    st: "state"
    l: "city"
    o: "Local Ansible"
    ou: "My Local Ansible Intermediate CA 2"
    email_address: "contact@your.domain.tld"
    ca_password: "jdieldodkiekie"
    password: "kdidkodkdokldkdokeok"
    certification_ca: "My Local Ansible Intermediate CA 1"

inv_bootstrap_ssl_files__end_certs:
  - cn: "my-end-certificate-1.domain.tld"
    c: "FR"
    st: "state"
    l: "city"
    o: "Local Ansible"
    ou: "Local Dev IT"
    email_address: "contact@your.domain.tld"
    alternatives:
      - "127.0.0.1"
      - "localhost"
      - "my-website.tld"
    certification_ca: "My Local Ansible Intermediate CA 1"

  - cn: "my-end-certificate-2.domain.tld"
    c: "FR"
    st: "state"
    l: "city"
    o: "My Local Intermediate CA Two"
    ou: "Local Dev IT"
    email_address: "contact@your.domain.tld"
    alternatives:
      - "127.0.0.1"
      - "localhost"
      - "my-website.tld"
    certification_ca: "My Local Ansible Intermediate CA 2"

  - cn: "my-end-certificate-3.domain.tld"
    c: "FR"
    st: "state"
    l: "city"
    o: "Local Ansible"
    ou: "Local Dev IT"
    email_address: "contact@your.domain.tld"
    alternatives:
      - "127.0.0.1"
      - "localhost"
      - "my-website.tld"
    certification_ca: "My Local Ansible Intermediate CA 2"

```

```YAML
# From AWX / Tower
---

```

### Run

To run this role, you can copy the molecule/default/converge.yml playbook and add it into your playbook:

```YAML
- name: "Include tool.bootstrap_ssl_files"
  tags:
    - "tool.bootstrap_ssl_files"
  vars:
    bootstrap_ssl_files__user: "{{ inv_bootstrap_ssl_files__user }}"
    bootstrap_ssl_files__base_path: "{{ inv_bootstrap_ssl_files__base_path }}"
    bootstrap_ssl_files__ca_validity: "{{ inv_bootstrap_ssl_files__ca_validity }}"
    bootstrap_ssl_files__key_size: "{{ inv_bootstrap_ssl_files__key_size }}"
    bootstrap_ssl_files__root_ca: "{{ inv_bootstrap_ssl_files__root_ca }}"
    bootstrap_ssl_files__intermediates_ca: "{{ inv_bootstrap_ssl_files__intermediates_ca }}"
    bootstrap_ssl_files__end_certs: "{{ inv_bootstrap_ssl_files__end_certs }}"
    bootstrap_ssl_files__cert_validity: "{{ inv_bootstrap_ssl_files__cert_validity }}"
  ansible.builtin.include_role:
    name: "tool.bootstrap_ssl_files"
```

## Architectural Decisions Records

Here you can put your change to keep a trace of your work and decisions.

### 2023-05-04: First Init

* First init of this role with the bootstrap_role playbook by Lord Robin Crombez (see [here](https://youtu.be/T_BbKnzYfP0))

### 2023-04-20: P12 Files

* Now the role can create P12 files, password protected
* Removed the boolean indicator in order to create a full bundle of file
* Probably need a CA ? Maybe it will be added ...

### 2023-05-27: CA Authorities and bundle

* Now we can create a CA Authority to sign a cert
* We can do that with "bootstrap_ssl_files__ca_name", "bootstrap_ssl_files__ca" and "bootstrap_ssl_files__cert" vars
* "bootstrap_ssl_files__ca" + "bootstrap_ssl_files__ca_name" => Create CA
* "bootstrap_ssl_files__name" + "bootstrap_ssl_files__cert" => Create CERT and sign with the CA from "bootstrap_ssl_files__ca_file"
* "bootstrap_ssl_files__cert" just create CERT
* Role make zip bundles (all files for a CA and for CERT) in a defined location by "bootstrap_ssl_files__dest"

### 2023-05-30: JKS file

* jks file added
* jks is convert in pkcs12 format

### 2023-05-30: PKC8 Key file

* file.kcs8.key file added, because some Java apps doesn't work with pkcs12

### 2023-06-28: Open source and refactoring

* Role dont create P12/P8/JKS files, because its a better practise to create desired files directly in the deployment role

### 2023-07-23: CA chains reworked, BREAKING CHANGES

* You can now define a totally custom PKI plateform
* You can create multile intermediaite CA with the level that you want (999 deep lenght max)
* Certificates can have any level of CA validation (ROOT, CA 1, CA 2 etc)
* CAs keys are password protected
* To deploy a cert, you have the ca-chain.pem.crt, that contains all cert of the structure for that cert
* Revocation system not implemented

### 2023-07-23: Revoke cert

* Role handle the revocation of certs if exist

### 2023-10-06: New CICD, new Images

* New CI/CD scenario name
* Molecule now use remote Docker image by Lord Robin Crombez
* Molecule now use custom Docker image in CI/CD by env vars
* New CICD with needs and optimization

## Authors

* Lord Robin Crombez

## Sources

* [Ansible role documentation](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html)
* [Ansible Molecule documentation](https://molecule.readthedocs.io/)
