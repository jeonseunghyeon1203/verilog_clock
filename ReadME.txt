202020984 전승현
input :clk(1kHZ), n_reset, mode(버튼1), inc(버튼2),bot3(버튼3),
       BUS1(버스스위치1)
output: [7:0]segdata( FND 2~5), [6:0]segdata2 (FND1), [3:0]segcom
        piezo_out,led10~0,

기능 : 기본 워치 모드 (1초마다 dot)
          AMPM워치 모드 
          시*분 설정모드 (blink)
          스탑워치 모드 
          알람 시*분 설정모드 
          타이머 시*분*초 설정모드 
          타이머 동작모드 
          설정모드에서 2초동안 누를시 고속카운트 d200(0.2초)->d50(0.05초)
          알람모드에서 알람 온오프
                 타이머나 알람소리 울릴때 소리끄기      
 
동작: 버튼1->모드 변경 
         버튼2->설정모드에선 증가버튼/스탑워치 타이머 모드에선 시작*멈춤버튼
                    /타이머==0이거나 알람 시간과 맞아 소리가 울릴때 끄는버튼 
         버튼3->스탑워치,타이머만 초기화, 알람모드일때 알람온오프 

모듈 설명: 
         WT_Sep(자리수 나누기), FND반환, AMPM반환모듈 사용 
 
         기본 워치모드 + 시간설정모드 -> MIN HOUR 
          스탑워치 모드 ->S_MSEC S_SEC S_MIN
          알람설정모드 -> A_MIN A_HOUR 
          타이머 설정모드 +타이머 동작모드 -> T_SEC T_MIN T_HOUR 
          
          모드마다 나눠 저장된 이 변수들을 [6:0]WT_MIN과 [6:0]WT_HOUR 저장
          WT_SEP모듈에서 FND한자리씩 표시할수있도록 자리수를 나눔 

          모든 설정모드의 클럭: 200clk 5HZ(0.2초) 
                 블링크 클럭: 500clk 2HZ(0.5초)
                 타이머 동작 클럭: posedge로 T_SEC의 첫비트가 0->1로 변할때 

                 피에조: T_Sound=1 이거나 Alarm=1이거나 버튼키가 눌렸을때 
                            단, 이때 버튼 2를 누르면 소리가 꺼짐 
                  T_Zero역할: 타이머가 모두 0이면 타이머 동작을 멈춤 
       AP 역할: FND1에 들어가는 0~9 또는 A/P, 한자리이므로  자리수 나누기 필요X
              
                 