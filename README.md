Instaling SLURM cluster manager using vagrant
=============================================

> Fork from http://mussolblog.wordpress.com/
>
> http://mussolblog.wordpress.com/2013/07/17/setting-up-a-testing-slurm-cluster/

Post-install setup
------------------

1. `vagrant up`

In the controller:

1. `vagrant ssh controller`
2. `sudo /usr/sbin/create-munge-key`
3. `sudo scp /etc/munge/munge.key vagrant@server:/home/vagrant/`
4. `sudo /etc/init.d/munge start`
5. `sudo slurmctld -D &`

In the server:

1. `vagrant ssh server`
2. `sudo cp ~/munge.key /etc/munge`
3. `sudo chown munge /etc/munge/munge.key`
4. `sudo /etc/init.d/munge start`
5. `sudo /etc/init.d/slurm-llnl start`

To test:

```shell
#!/bin/bash
#SBATCH -p debug
#SBATCH -n 1
#SBATCH -t 12:00:00
#SBATCH -J some_job_name
 
ls / > /home/vagrant/slurm.out
```

`sbatch test.sh`
