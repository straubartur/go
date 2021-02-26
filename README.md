# go
artur crud
```golang
package main

import (
    "net/http"
	"encoding/json"
	"fmt"
	"github.com/gorilla/mux"
)
type PostList struct {
	Postlist []Post
}

type Post struct{
	Title string
	Content string
}

func getPostList() PostList {
	a := Post{"artur1", "arturcontent"}
	b := Post{"artur1", "arturcontent"}
	fmt.Println(a)
	posts := []Post{a, b}
	postList := PostList{
		posts,
	}
	return postList
}

var postList PostList
func getPostLists(w http.ResponseWriter, r *http.Request) {
	w.Header().Set("Content-Type", "application/json")

	js, err := json.Marshal(postList)
	if err != nil {
		http.Error(w, err.Error(), http.StatusInternalServerError)
		return
	}
	w.Header().Set("Content-Type", "application/json")
	w.Write(js)
}

func (postList *PostList) insertPost(post Post) {
	fmt.Println(post)
	postList.Postlist = append(postList.Postlist, post)
	return
}
func createPost(w http.ResponseWriter, r *http.Request) {
	var post Post
	w.Header().Set("Content-Type", "application/json")

	js := json.NewDecoder(r.Body).Decode(&post)
	postList.insertPost(post)
	fmt.Println(js, post)
	fmt.Fprint(w, "Criado com sucesso")
} 
func handleRequests() {
	myRouter := mux.NewRouter().StrictSlash(true)
	myRouter.HandleFunc("/posts", getPostLists)
	myRouter.HandleFunc("/posts2", createPost).Methods("POST")
	
	http.ListenAndServe(":8000", myRouter)
}
func main() {
	
	postList = getPostList()
	handleRequests()
}
```
