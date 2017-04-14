.. _infrastructure:


Intro
@@@@@

Our infrastructure is primarily AWS-based, combined with best-in-class tools where they are substantially better than AWS tooling or where AWS has gaps. For example, we might use Kubernetes for Docker container orchestration as opposed to AWS EC2 container service.

AWS Services
@@@@@@@@@@@@

We use the following AWS services:

* EC2: Compute

* OS: Amazon EC2 Linux

* RDS: Storage

* SQL Database: PostgreSQL (default structured), MySQL (when required)

* NoSQL Database: MongoDB (TBD)