# Book catalog schema exercise


## Exercise

### Part 1

Imagine we're building an app that stores information about books, somewhat like a simplified version of https://www.goodreads.com/

You are to design the database schema for such a system, to meet the following requirements:

- Each book has an author. Some books have multiple authors.
- Each author has a name and biography
- Some books are print-only. Some are audio-only. And some are both.
- Print and audio books always have the same: author(s), titles, synopses
- Print and audio books can have different: ISBNs, and publication dates
- Print books have page counts -- audio books don't.
- Audio books have: running time, and one or more narrators -- print books don't.
- Like authors, narrators also have names and biographies.

For the purposes of this exercise ignore the following additional complexities:

- Books in different languages
- Books published multiple times in separate editions
- Series which contain multiple books
- Other data like genres, ratings, etc.

schema design in yaml format:

```yaml
- artist:
    - id
    - name 
    - email # unique
    - biography
    - role  - # author - 0, narrator - 1, both - 2


- author_book:
    - id
    - artist_id # FK to artist.id
    - book_id # FK to book.id

- narrator_book:
    - id
    - artist_id # FK to artist.id
    - book_media_id # FK to book_media.id

- book:
    - id
    - title #unique
    - synopse

- book_media-:
    - id
    - book_id # FK to book.id
    - type # print,audio
    - ISBN
    - publication_date
    - page_count # null=True,blank=True
    - running_time # null=True,blank=True
    - is_author_narrator_same # boolean deflaut  - False  
    

Additional comments to explain field types, nullability, indexes, and constraints when important or not obvious.

### Part 2

Given your schema, now we'd like to write some SQL to find authors who have all of their audio books narrated by themselves.

SELECT artist.id,artist.name FROM  artist 
INNER JOIN author_book ON author_book.artist_id = artist.id 
INNER JOIN book ON author_book.book_id = book.id 
INNER JOIN book_media ON book.id  = book_media.book_id 
WHERE book_media.is_author_narrator_same = 1;

