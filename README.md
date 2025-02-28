# MVCC (Multi-Version Concurency Control)
```
Multi-version Concurency control allows multiple transactions to take place simultaneously while maintaining data consistency and isolation.
Multiple transaction can happen with database at the same time without blocking each other. It maintains multiple versions of rows.
- PostgreSQL support three columns internally to track rows versions (Xmin, Xmax, Xid).
- When transaction starts, PostgreSQL assigns a Transaction ID (XID) to that transaction.
- When transaction updated or Deleted postgresql assigns a transaction Id to (Xmax),
- When transaction inserted postgresql assigns a transaction id to (Xmin).

1.Starts transaction -> Assigns XID
2.Read Data -> sees only committed rows before its XID.
3.Modify Data -> Creates a new version (tuple) of the row with updated data & old version is marked as dead both versions arepresent until the vacuum process cleans up the dead row.
4.Commit Transaction -> New row version becomes visible.
5.RollBack Transaction -> New row version is discarded.
6.Delete -> The row is only marked as dead, no new version is created.

```
#### When will transaction Block & which Transaction will Not Block?
```
# Transaction will Block.
1.UPDATE : Row-Level Lock
2.DELETE : Row-Level Lock
--------------------------------
# Transaction Will not Block.
1.SELECT : Reads older version
2.INSERT on different Rows
3.UPDATE on Different Rows
4.DELETE on Dfferent Rows
```

#### If we want 100% non-blocking reads use :
```
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;
```

```
- SERIALIZABLE gurantees that transaction will execute in a way as if they were executed one by one sequentlly, even id they actually run concurently.
- Postgresql creates snapshots of the database at the moment the transaction starts.
- transaction reads only the snapshot data.
- It will not see any changes made by other concurrent transaction until it commits.
- If two transaction try to modify the same row or related data, one transaction will autimatically abort with an error: "could not serialize access due to concurrent update".

-------------------------

Use "NOWAIT" The query will imidiatly returns an error if the row is already locked.
```



