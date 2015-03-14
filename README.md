PostgreSQL GDB commands
=======================

GDB commands making debugging PostgreSQL internals easier. Currently
this provides a single command 'pgprint' that understand the 'Node'
structures used internally, can can print them semi-intelligently.


Usage
-----

Copy the `gdbpg.py` script somewhere, and load it from `gdb`:

    $ gdb ... 
    (gdb) source /...path.../gdbpg.py 

and then just use `pgprint` command to print variables from gdb console.
E.g. if `plan` is pointing to `(PlannedStmt *)`, you may do this:
    
    (gdb) pgprint plan

and you'll get something like this:

              type: CMD_SELECT
          query ID: 0
        param exec: 0
         returning: False
     modifying CTE: False
       can set tag: True
         transient: False
      row security: False
               
         plan tree: 
            -> HashJoin (cost=202.125...342.812 rows=2550 width=16)
                    target list:
                            TargetEntry (resno=1 resname="id" ...
                            TargetEntry (resno=2 resname="id" ...
                            TargetEntry (resno=3 resname="id" ...
                            TargetEntry (resno=4 resname="id" ...
                    ...

Limitations
-----------

Not all `Node` types are supported (only the subset I recently needed).
It's rather trivial to add support for more nodes though - just hack the
`format_node` function a bit.
