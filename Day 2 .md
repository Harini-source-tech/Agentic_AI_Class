OPS = {">=": operator.ge, "<=": operator.le, "==": operator.eq,
       "in": lambda actual, allowed: actual in allowed.split(",")}
 
def check_eligibility(student_id: str, drive_id: int) -> dict:
    """(the strong description from block B)"""
    student = repo.students.get(student_id)
    if student is None:
        return {"error": "unknown_student",
                "hint": "Ask the user for their roll number."}
 
    failed = []
    for rule in repo.rules.for_drive(drive_id):
        actual = getattr(student, rule.field)
        if not OPS[rule.op](actual, rule.typed_value()):
            failed.append({"rule_id": rule.id,
                           "rule": str(rule),
                           "actual": actual})
 
    return {"eligible": not failed, "failed_rules": failed}


    Day 2 Keypoints:
    run 
