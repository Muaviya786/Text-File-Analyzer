# Text-File-Analyzer

import string
from collections import Counter
import os

def get_text_statistics(file_path, top_n=10):
    word_count = 0
    line_count = 0
    words = []

    with open(file_path, 'r', encoding='utf-8') as file:
        for line in file:
            line_count += 1
            line_cleaned = line.translate(str.maketrans('', '', string.punctuation)).lower()
            line_words = line_cleaned.split()
            word_count += len(line_words)
            words.extend(line_words)

    word_frequencies = Counter(words)
    most_common_words = word_frequencies.most_common(top_n)

    return {
        "word_count": word_count,
        "line_count": line_count,
        "most_frequent_words": most_common_words
    }

if __name__ == "__main__":
    folder_path = input("Enter the folder path: ")
    file_name = input("Enter the file name (without '.txt'): ") + ".txt"
    file_path = os.path.join(folder_path, file_name)
    
    if os.path.exists(file_path):
        stats = get_text_statistics(file_path)
    
        print(f"\nStatistics for {file_name}:\n")
        print(f"Total word count: {stats['word_count']}")
        print(f"Total line count: {stats['line_count']}")
        print("Most frequent words:")
        for word, freq in stats['most_frequent_words']:
            print(f"'{word}': {freq} times")
    else:
        print("Error: File not found. Please check the folder path and file name.")
