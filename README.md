package main

import (
	"net/http"

	"github.com/gin-gonic/gin"
)

type Verse struct {
	ID       string `json:"id"`
	Verse    string `json:"verse"`
	Quantity uint   `json:"quantity"`
}

var Verses = []Verse{
	{"1", "God loves the world", 3},
	{"2", "He comes with the clouds", 5},
}

func main() {
	router := gin.Default()
	router.GET("/", handleRoot)
	router.GET("/:verse", handleRequest)
	router.POST("/:verse/*path", handleRequest)
	router.Run("localhost:8080")
}

func handleRoot(c *gin.Context) {
	c.JSON(http.StatusOK, gin.H{
		"message": "Welcome to the root path!",
	})
}

func handleRequest(c *gin.Context) {
	verse := c.Param("verse")
	path := c.Param("path")

	if verse == "" {
		c.JSON(http.StatusBadRequest, gin.H{
			"error": "Verse parameter is required",
		})
		return
	}

	// Handle both GET and POST requests with the dynamic parameter and wildcard path
	// You can add logic here to retrieve or manipulate the data based on the parameters

	c.JSON(http.StatusOK, gin.H{
		"message": "Request for verse: " + verse + ", path: " + path,
	})
}
