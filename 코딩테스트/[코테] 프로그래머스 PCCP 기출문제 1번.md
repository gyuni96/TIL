// ## 프로그래머스 PCCP 기출문제 1번
// ## 난이도 : Lv.1
// ## https://school.programmers.co.kr/learn/courses/30/lessons/250137
// ## 
// ## 문제
// ## 붕대감기라는 기술이 사용하면서 몬스터에 공격패턴을 끝까지 버틸수 있는지 여부를 구하는 로직을 구현한다.
// ## 1. 붕대감기는 t초동안 시전하고 1초마다 x의 체력을 회복하고 연속으로 t초만큼 성공하면 y만큼 추가 회복을한다.
// ## 2. 최대 체력이 정해져있고 그이상 회복하는것은 불가능
// ## 3. 몬스터한테 공격을 받으면 체력이 줄어들고 기술이 취소됨
// ## 
// ## 제한사항
// ## bandage는 [시전 시간, 초당 회복량, 추가 회복량] 형태의 길이가 3인 정수 배열입니다.
// ## 1 ≤ 시전 시간 = t ≤ 50
// ## 1 ≤ 초당 회복량 = x ≤ 100
// ## 1 ≤ 추가 회복량 = y ≤ 100
// ## 1 ≤ health ≤ 1,000
// ## 1 ≤ attacks의 길이 ≤ 100
// ## attacks[i]는 [공격 시간, 피해량] 형태의 길이가 2인 정수 배열입니다.
// ## attacks는 공격 시간을 기준으로 오름차순 정렬된 상태입니다.
// ## attacks의 공격 시간은 모두 다릅니다.
// ## 1 ≤ 공격 시간 ≤ 1,000
// ## 1 ≤ 피해량 ≤ 100
// ## 
// ## 구현사항
// ## 현재 체력 체크
// ## 0 이하일경우 -1로 리턴
// ## 공격 여부 체크
// ## 공격이 있을 경우 데미지
// ## 공격이 없을 경우 회복
// ## 연속 회복 성공 상태 ( 초기화 or 추가)
// ## 연속 회복 성공 체크
// ## 추가 회복
function solution(bandage, health, attacks) {
    
  const attackEndTime = attacks.at(-1)[0] // 총 진행시간
  let currentHealth = health // 현재 체력
  
  const attackMap = new Map(attacks)
  //const attacTime = attacks.map(el=>el[0]) // 공격시간
  //const attacDmg = attacks.map(el=>el[1]) // 공객 데미지
  // 상수를 두개 만드는거보다 Map Object를 이용해서 상수 최소화
  
  let continuityCnt = 0 // 연속 회복 횟수
  
  for(let i = 1 ; attackEndTime >= i ; i++){
        if(attackMap.has(i)){
        //if(attacTime.indexOf(i) > 0){
        // 공격이 있는 시간일 경우
          currentHealth -= attackMap.get(i) // 데미지
          //currentHealth -= attacDmg[attacTime.indexOf(i)] // 데미지
          continuityCnt = 0 // 연속 회복 초기화
        }else{
          // 공격이 없는 시간일 경우
          currentHealth += bandage[1] // 회복
          continuityCnt++ // 연속 회복 +1

          if(continuityCnt === bandage[0]){
            // 연속회복 성공시 추가 회복
            currentHealth += bandage[2] // 추가회복
            continuityCnt = 0 // 초기화
          }
          if(currentHealth > health){
            // 회복해줄때만 비교로 수정
            // 최대체력보다 많을시 초기화
            currentHealth = health
          }
        }

        // 더 자주 실행되는 함수부터 순서 최적화
        if(currentHealth <= 0){
          // 현재체력이 0이면 -1 리턴
          return -1
        }
    }
    return currentHealth
}
