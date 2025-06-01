#### Задача 1

Какой будет результат запуска тестового метода executeTest()?

```java
class MissionCompleteTest {

	@Test
	public void executeTest() {
		MissionComplete mission = new MissionComplete();
		Mockito.when(mission.execute()).thenReturn(true);	
		boolean result = mission.execute();
		assertEquals(true, result);
	}
}
```


#### Задача 2

Какой будет результат запуска тестового метода executeTest()?

```java
@ExtendWith(MockitoExtension.class)
class JoinToBattleTest {

	@InjectMocks
	private JoinToBattle joinToBattle;
	@Mock
	PvpBattle pvpBattle;
	@Mock
	ClientSession clientSession;
	
	@Test
	public void executeTest() {
		int sessionId = 08045444855;
		int battleId = 88005553535;
		List<Player> players = new ArrayList<> {{
			add(new Player());
			add(new Player());
		}};
		
		Mockito.when(clientSession.getSessionId()).thenReturn(sessionId);
		Mockito.when(pvpBattle.getBattleId()).thenReturn(battleId);
		Mockito.when(pvpBattle.getPlayers()).thenReturn(players);
		
		joinToBattle.process();
		
		verify(joinToBattle, times(1)).process();
	}
}
```