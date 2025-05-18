#include <stdio.h>
#include <string.h>
#include <ctype.h>

#define MAX_TEXT_LENGTH 1000
#define MAX_WORD_LENGTH 100
void to_lowercase(char *str) {
    for (int i = 0; str[i]; i++) {
        str[i] = tolower(str[i]);
    }
}
int is_word_in_list(const char *word, const char *word_list[], int list_size) {
    for (int i = 0; i < list_size; i++) {
        if (strcmp(word, word_list[i]) == 0) {
            return 1; 
        }
    }
    return 0; 
}
const char* analyze_sentiment(const char *text, const char *positive_words[], const char *negative_words[], int pos_size, int neg_size) {
    char word[MAX_WORD_LENGTH];
    int pos_count = 0, neg_count = 0;

    int i = 0, word_index = 0;
    while (text[i] != '\0') {
        if (isalnum(text[i])) {
            word[word_index++] = text[i];
        } else {
            if (word_index > 0) {
                word[word_index] = '\0'; 
                to_lowercase(word); 
                if (is_word_in_list(word, positive_words, pos_size)) {
                    pos_count++;
                } else if (is_word_in_list(word, negative_words, neg_size)) {
                    neg_count++;
                }

                word_index = 0; 
            }
        }
        i++;
    }
    if (word_index > 0) {
        word[word_index] = '\0';
        to_lowercase(word);
        if (is_word_in_list(word, positive_words, pos_size)) {
            pos_count++;
        } else if (is_word_in_list(word, negative_words, neg_size)) {
            neg_count++;
        }
    }
    if (pos_count > neg_count) {
        return "Positive";
    } else if (neg_count > pos_count) {
        return "Negative";
    } else {
        return "Neutral";
    }
}

int main() {
    const char *positive_words[] = {"good", "great", "happy", "awesome", "love", "excellent", "fantastic"};
    const char *negative_words[] = {"bad", "terrible", "hate", "horrible", "awful", "poor"};
    char text[MAX_TEXT_LENGTH];
    printf("Enter a sentence or paragraph for sentiment analysis:\n");
    fgets(text, MAX_TEXT_LENGTH, stdin);    text[strcspn(text, "\n")] = '\0'; 
        const char *sentiment = analyze_sentiment(text, positive_words, negative_words, 
                                              sizeof(positive_words)/sizeof(positive_words[0]), 
                                              sizeof(negative_words)/sizeof(negative_words[0]));
    printf("\nSentiment of the input text: %s\n", sentiment);

    return 0;
}
