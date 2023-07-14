#dsa #datastructures #algorithms #algorithmoptimization #queues 

In this note we will be taking a look at the most common application of the queue data structure; a song queue for media players like spotify. First, we will need a library of songs that we can add to the queue. A common way to handle this is with a dictionary like this:
```python
catalogue = {
	100: {
		"track_name": "song 1",
		"album_name": "album 1",
		"artist_name": "artist 1",
		"download_link": "https://www.blah/track/100"
		"length": 5
	},
		101: {
		"track_name": "song 2",
		"album_name": "album 2",
		"artist_name": "artist 2",
		"download_link": "https://www.blah/track/101"
		"length": 5
	},
		102: {
		"track_name": "song 3",
		"album_name": "album 3",
		"artist_name": "artist 3",
		"download_link": "https://www.blah/track/102"
		"length": 5
	}
}
```
Now that we have our catalogue of songs, we can add them to our music players queue using their ids. For the purposes of this example we will be creating track queue class that will inherit from the queue class in this note: [[Data Structures - Queues]].

```python
class TrackQueue(Queue):
	def __init__(self):
		super(TrackQueue, self).__init__()

	def add_track(self, track):
		self.enqueue(track)

	def cycle(self):
		while self.q.count > 0:
			current_track_metadata = self.dequeue()
			current_track = self.download(current_track_metadata)
			self.play(current_track)

	def download(self, track_data):
		print(f"Downloading {track_data["track_name"]} from {track_data["download_link"]}")
		return track_data

	def play(self, track):
		print(f"Playing {track["track_name"]} from {track["artist_name"]})
		time.sleep(track["length"])
		#The sleep here is to simulate playing the song
```

Notice that we have inherited all the basic methods of a queue from the parent class and used them to implement methods like cycle to download the next song, play it and repeat, and add_track, which just calls enqueue but has a method name that suits the context more and is thus more intuitive.