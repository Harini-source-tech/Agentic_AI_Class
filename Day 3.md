def enqueue(self, thread_id, text, model, max_attempts=3):
    run_id = str(uuid.uuid4())
    with self.transaction() as c:        # message and run: both or neither
        self.append_message(thread_id, "user", text)
        c.execute("INSERT INTO run (id, thread_id, status, model,"
                  " max_attempts, available_at)"
                  " VALUES (?, ?, 'queued', ?, ?, ?)",
                  (run_id, thread_id, model, max_attempts, self.clock()))
    return run_id
 
def claim_next(self, worker_id, lease_seconds):
    now = self.clock()
    with self.transaction() as c:        # BEGIN IMMEDIATE
        row = c.execute(
            "SELECT id, thread_id, attempts FROM run"
            " WHERE status = 'queued' AND available_at <= ?"
            " ORDER BY available_at, created_at LIMIT 1",
            (now,)).fetchone()
        if row is None:
            return None
        c.execute("UPDATE run SET status = 'running', lease_owner = ?,"
                  " lease_until = ?, attempts = attempts + 1 WHERE id = ?",
                  (worker_id, now + lease_seconds, row["id"]))
        return Claimed(row["id"], row["thread_id"], row["attempts"] + 1)



        Day 3- keypoints
        enqueue
        heartbeat
        Idopotency queue
        Checkpoint 
