# `bash`  One-Liner Gists

A flight of miscellaneous `bash` snippets that I consistently use but can never remember.

## `SLURM` Jobs Statistic Summary

This one-liner displays the accounting data for all `SLURM` jobs in the specified time interval.

```bash
# Display the SLURM job summary for all jobs submitted between midnight and now.
sacct -o jobid,jobname,submit,state,elapsed,reqmem,maxrss -S 00:00:00 -E now -u dpeede --units=G
```

The `sacct` command displays job accounting data stored in the job accounting log file or `SLURM` database in a variety  of  forms  for  your analysis.   The  `sacct` command displays information on jobs, job steps, status, and exitcodes by default.

- `sacct`: Displays accounting data for all jobs and job steps in the `SLURM`  job accounting log or `SLURM` database.
- `-o jobid,jobname,submit,state,elapsed,reqmem,maxrss`: `-o` lists the fields to be displayed in the output, separated by commas.
	- `jobid`: Job ID. 
	- `jobname`: Name of the job.
	- `submit`: Submission time of the job.
	- `state`: Final state of the job—e.g., `COMPLETED`, `FAILED`.
	- `elapsed`: Elapsed time the job ran.
	- `reqmem`: Memory requested by the job.
	- `maxrss`: Maximum Resident Set Size of the job—i.e., the maximum amount of memory used.
- `-S 00:00:00`: `-S` start time for the report. This specifies the beginning of the time range for which to display jobs.
	- `00:00:00`: Here, it's set to midnight—start of the current day.
	- Note, you can specify start dates in `YYYY-MM-DD` format, or times like `HH:MM[:SS]` for more precise start times.
- `-E now`: `-E` end time for the report. This specifies the end of the time range for which to display jobs.
	- `now`: Here, it's set to the time it is at the execution of the command—now means the current time.
	- Note, you can specify end dates in `YYYY-MM-DD` format, or times like `HH:MM[:SS]` for more precise end times.
- `-u dpeede`: `-u` user filter. This option limits the report to jobs submitted by the user `dpeede`.
- `--units=G`: `--units` specifies the units for memory-related fields. `G` stands for gigabytes, which affects how memory usage—e.g.,  `reqmem` and `maxrss`—is displayed.

