Instaling SLURM cluster manager using vagrant
=============================================

> Fork from http://mussolblog.wordpress.com/
>
> http://mussolblog.wordpress.com/2013/07/17/setting-up-a-testing-slurm-cluster/

Post-install setup
------------------

1. `vagrant up`

2. In the controller:

   ```
   vagrant ssh controller
   sudo /usr/sbin/create-munge-key
   sudo scp /etc/munge/munge.key vagrant@server:/home/vagrant/
   sudo /etc/init.d/munge start
   sudo slurmctld -D &
   ```

3. In the server:

   ```
   vagrant ssh server
   sudo cp ~/munge.key /etc/munge
   sudo chown munge /etc/munge/munge.key
   sudo /etc/init.d/munge start
   sudo /etc/init.d/slurm-llnl start
   ```

4. To test:

   ```shell
   #!/bin/bash
   #SBATCH -p debug
   #SBATCH -n 1
   #SBATCH -t 12:00:00
   #SBATCH -J some_job_name
    
   ls / > /home/vagrant/slurm.out
   ```
   
   `sbatch test.sh`
