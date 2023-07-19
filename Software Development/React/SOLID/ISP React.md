Created on 2023-07-16 16:31 

[[Interface segregation principle]]

Example: 

```jsx
type Video = {
    title: string
    duration: number
    coverUrl: string
}

type Props = {
    items: Array<Video>
}

type PropsT = {
	video: Video
}

const Thumbnail = ({ video }: PropsT) => {
	return <img src={video.coverUrl} />
}

const VideoList = ({ items }) => {
    return (
        <ul>
            {items.map((item) => (
                <Thumbnail key={item.title} video={item} />
            ))}
        </ul>
    );
};

```

Applying ISP:

```jsx
type LiveStream = {
	name: string
	previewUrl: string
}

type Props = {
	items: Array<Video | LiveStream>
}

type PropsT = {
	coverUrl: string
}
const Thumbnail = ({ coverUrl }: PropsT) => {
	return <img src={coverUrl} />
}

const VideoList = ({ items }: Props) => {
	return (
		<ul>
			{items.map(item => {
				if ('coverUrl' in item) {
					// it's a video
					return <Thumbnail coverUrl={item.coverUrl} />
				} else {
					// it's a live stream
					return <Thumbnail coverUrl={item.previewUrl} />
				}
			})}
		</ul>
	)
}

```

The ISP, advocates for minimizing dependencies between the components of the system, making them less coupled and thus more reusable.