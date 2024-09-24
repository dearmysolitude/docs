##  useEffect
외부 시스템과 컴포넌트를 동기화하는 React Hook.
### "mount"
- compound는 DOM Tree에 의해 구조가 정리되고, 그려질 때 컴파운더가 추가되면 mount,  제거되면 unmount된다.
- 함수형으로 작성이 되기 전에는 js의 클래스객체를 사용해  didMount  / willMount로 mount하였다.
### useEffect
- Setup Code: mount 시 1회 호출
- Cleanup Code: unmount 시 1회 호출
- dependencies 에 속한 state가 변경될 때 useEffect가 n회 호출된다. 
```javascript
useEffct(() => {
	const connection = createConnection(serverUrl, roomId);
	connection.connect();
	return () => {
		connection.disconnect();
	};
}, [serverUrl, roomId]);
```
2 ~ 3 라인: Setup Code
5 라인: Cleanup Code
7 라인: list of dependency

uesEffect예시: 실행해보면 status 토글을 클릭할 때마다 TODO list와 Done list를 불러온다.
```js
export default function AppEffects() {
  const [status, setStatus] = useState("TODO");
  const [dataList, setDataList] = useState([]);

  useEffect(() => {
    fetch(`data/${status === "TODO" ? "todoList.json" : "doneList.json"}`)
      .then((res) => res.json())
      .then((data) => {
        console.log("data : ", data);
        setDataList(data);
      });
  }, [status]);
  
  const handleClick = () => {
    setStatus(status === "TODO" ? "DONE" : "TODO");
  };

  return (
    <section style={{ width: '60%', padding: '15px' }}>
      <div style={{ display: "flex", justifyContent: 'space-between', alignItems: 'center', padding: '15px' }}>
        <h2> {status} list </h2>
          <button onClick={handleClick} style={{ width: '60px', height: '20px', textAlign: 'center' }}> Toggle </button>
      </div>
      {dataList.map((item) => (
        <MyItem key={item.id} item={item} />
      ))}
    </section>
  );
}
```