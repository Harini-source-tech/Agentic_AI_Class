22CS045> Is Designing Data-Intensive Applications available?
         If it is, reserve it for me and send me a text.
  supervisor → ask_catalogue({"question": "Is Designing ... available?"})
      catalogue → search_books({"text": "Data-Intensive"})
                ← {"books": [{"book_id": 2, ..., "copies_available": 1}]}
             ← "Book 2 ... 1 copy available."
  supervisor → ask_desk({"request": "Reserve book 2 ... text the member"})
      desk → check_can_borrow({})
           ← {"can_borrow": true, "reasons": []}
      desk → reserve_book({"book_id": 2})
           ← {"book_id": 2, "status": "reserved"}
      desk → notify_member({"message": "Your reservation ... confirmed."})
           ← {"notification_id": 1, "status": "queued"}
             ← "Reserved book 2 and sent the confirmation."
assistant> Good news: it was available, it's now reserved for you ...
agent 
rag
patterns
