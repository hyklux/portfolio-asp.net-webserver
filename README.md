# portfolio-asp.net-webserver
ASP.NET 웹서버 포트폴리오

## 소개
REST API를 활용해 게임 결과 순위에 대한 CRUD를 수행하는 웹기반 API 서버입니다.

## 기능
:heavy_check_mark: 엔티티용 클래스 GameResult 생성


:heavy_check_mark: ApplicationDbContext 생성 및 GameResults DbSet 선언


:heavy_check_mark: REST API에 대한 응답을 처리할 Controller 생성


:heavy_check_mark: 게임 결과 불러오기(Read)


:heavy_check_mark: 게임 결과 생성하기(Create)


:heavy_check_mark: 게임 결과 수정하기(Update)


:heavy_check_mark: 게임 결과 삭제하기(Delete)


## 엔티티용 클래스 GameResult 생성
``` c#
namespace SharedData.Models
{
	public class GameResult
	{
		public int Id { get; set; }
		public int UserId { get; set; }
		public string UserName { get; set; }
		public int Score { get; set; }
		public DateTime Date { get; set; }
	}
}
```

(DB 사진 추가)
## ApplicationDbContext 생성 및 GameResults DbSet 선언
``` c#
public class ApplicationDbContext : DbContext
{
	public DbSet<GameResult> GameResults { get; set; }

	public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
		: base(options)
	{

	}
}
```


## REST API에 대한 응답을 처리할 Controller 생성
``` c#
[Route("api/[controller]")]
[ApiController]
public class RankingController : ControllerBase
{
	ApplicationDbContext _context;

	public RankingController(ApplicationDbContext context)
	{
		_context = context;
	}
}
```


## 게임 결과 불러오기(Read)
``` c#
// Read
// 모든 아이템
// GET /api/ranking
[HttpGet]
public List<GameResult> GetGameResults()
{
	List<GameResult> results = _context.GameResults
		.OrderByDescending(item => item.Score)
		.ToList();

	return results;
}
```
![asp net_webserver_read1](https://user-images.githubusercontent.com/96270683/222903540-ad44db2b-16ca-48a9-a605-503b0351c332.PNG)


## 게임 결과 생성하기(Create)
``` c#
// Create
// 아이템 생성 요청 (Body에 실제 정보)
// POST /api/ranking
[HttpPost]
public GameResult AddGameResult([FromBody] GameResult gameResult)
{
	_context.GameResults.Add(gameResult);
	_context.SaveChanges();

	return gameResult;
}
```
![asp net_webserver_create1](https://user-images.githubusercontent.com/96270683/222903547-d8019cce-d42c-41cb-bd8d-bd98be22f7d7.PNG)
![asp net_webserver_create2](https://user-images.githubusercontent.com/96270683/222903552-43362467-f645-4c41-b7b4-ced2ad1e8667.PNG)


## 게임 결과 수정하기(Update)
``` c#
// Update
// 아이템 갱신 요청 (Body에 실제 정보)
// PUT /api/ranking (PUT 보안 문제로 웹에선 활용 X)
[HttpPut]
public bool UpdateGameResult([FromBody] GameResult gameResult)
{
	var findResult = _context.GameResults
		.Where(x => x.Id == gameResult.Id)
		.FirstOrDefault();

	if (findResult == null)
		return false;

	findResult.UserName = gameResult.UserName;
	findResult.Score = gameResult.Score;
	_context.SaveChanges();

	return true;
}
```
![asp net_webserver_update1](https://user-images.githubusercontent.com/96270683/222903559-153a85dc-1d03-4543-a265-d0b7e7f26e6d.PNG)
![asp net_webserver_update2](https://user-images.githubusercontent.com/96270683/222903563-98f096b8-1011-41b2-88fd-8d823a5e0f85.PNG)


## 게임 결과 삭제하기(Delete)
``` c#
// Delete
// id=1번인 아이템 삭제
// DELETE /api/ranking/1 (DELETE 보안 문제로 웹에서 활용 X)
[HttpDelete("{id}")]
public bool DeleteGameResult(int id)
{
	var findResult = _context.GameResults
				.Where(x => x.Id == id)
				.FirstOrDefault();

	if (findResult == null)
		return false;

	_context.GameResults.Remove(findResult);
	_context.SaveChanges();

	return true;
}
```
