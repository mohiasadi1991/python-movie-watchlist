
import jso
from pathlib import Path

FILE = Path("movies.json")

def load_movies():
    if FILE.exists():
        try:
            return json.loads(FILE.read_text(encoding="utf-8"))
        except json.JSONDecodeError:
            print("Warning: Invalid movie data.")
    return []

def save_movies(movies):
    FILE.write_text(
        json.dumps(movies, indent=4, ensure_ascii=False),
        encoding="utf-8"
    )

def add_movie(movies):
    title = input("Movie title: ").strip()
    genre = input("Genre: ").strip()

    if not title:
        print("Movie title cannot be empty.")
        return

    movies.append({
        "title": title,
        "genre": genre,
        "watched": False
    })

    save_movies(movies)
    print("Movie added successfully!")

def show_movies(movies):
    if not movies:
        print("Your watchlist is empty.")
        return

    print("\n=== MY WATCHLIST ===")

    for i, movie in enumerate(movies, 1):
        status = "Watched" if movie["watched"] else "Not watched"
        print(
            f"{i}. {movie['title']} | "
            f"{movie['genre']} | {status}"
        )

def search_movie(movies):
    keyword = input("Enter movie title: ").strip().lower()

    results = [
        movie for movie in movies
        if keyword in movie["title"].lower()
    ]

    if results:
        for movie in results:
            status = "Watched" if movie["watched"] else "Not watched"
            print(f"{movie['title']} - {movie['genre']} - {status}")
    else:
        print("No matching movies found.")

def mark_watched(movies):
    show_movies(movies)

    if not movies:
        return

    try:
        number = int(input("Movie number to mark as watched: "))

        if 1 <= number <= len(movies):
            movies[number - 1]["watched"] = True
            save_movies(movies)
            print("Movie marked as watched!")
        else:
            print("Invalid movie number.")

    except ValueError:
        print("Please enter a valid number.")

def show_stats(movies):
    total = len(movies)
    watched = sum(movie["watched"] for movie in movies)

    print(f"Total movies: {total}")
    print(f"Watched: {watched}")
    print(f"Remaining: {total - watched}")

def main():
    movies = load_movies()

    while True:
        print("\n=== PYTHON MOVIE WATCHLIST ===")
        print("1. Add movie")
        print("2. Show watchlist")
        print("3. Search movie")
        print("4. Mark as watched")
        print("5. Show statistics")
        print("6. Exit")

        choice = input("Choose an option: ").strip()

        if choice == "1":
            add_movie(movies)
        elif choice == "2":
            show_movies(movies)
        elif choice == "3":
            search_movie(movies)
        elif choice == "4":
            mark_watched(movies)
        elif choice == "5":
            show_stats(movies)
        elif choice == "6":
            print("Goodbye!")
            break
        else:
            print("Invalid choice. Try again.")

if __name__ == "__main__":
    main()
