# 테스트에 쓴 프롬프트 모음

공통 설정: 시드 `20260920` / 4스텝 / CFG 2.5 / 1024×1536 / euler·simple / 참조모드 `뼈대`
네거티브 프롬프트(전부 동일): `lowres, bad anatomy, extra limbs, deformed hands, worst quality`

## 제출본

| 번호 | 포즈 이미지 | 프롬프트 | 결과 이미지 | 메모 |
|---|---|---|---|---|
| 1 | samples/pose_01.png | `an astronaut in a white spacesuit, jumping with arms and legs spread wide, photorealistic, studio lighting, plain background` | samples/output_01.png | 자세가 그대로 옮겨졌다. 팔 V자·다리 역V자·공중 체공·펼친 손가락까지. 인물은 원본과 무관. 왼손 손가락이 4개로 생성된 결함 있음 |
| 2 | samples/pose_02.png | 1번과 동일 | samples/output_02.png | 몸 방향(측면)과 두 팔의 좌우는 따라왔으나 **두 다리의 좌우가 뒤바뀜**. 팔은 그대로여서 같은 쪽 팔다리가 함께 앞뒤로 가는, 사람이 실제로는 하지 않는 자세가 됨 |

3단 비교 이미지: `samples/compare_01.png`, `samples/compare_02.png` (왼쪽부터 참조 사진 / 뼈대 / 결과)

## 조건을 바꿔 본 실험

| 실험 | 고정한 것 | 바꾼 것 | 결과 |
|---|---|---|---|
| A | 포즈 1, 시드, 스텝, CFG | 프롬프트를 `a medieval knight in full plate armor, jumping with arms and legs spread wide, photorealistic, studio lighting, plain background` 로 | 자세 유지. 갑옷처럼 윤곽이 뻣뻣한 대상에서도 흔들리지 않았다. 덤으로 손가락 결함이 사라졌다 |
| B | 프롬프트(1번과 동일), 시드, 스텝, CFG | 포즈 사진을 pose_01(정면 점프) → pose_02(측면 달리기) 로 | 새 자세를 따라갔으나 좌우가 뒤바뀌었다. 프롬프트에 남아 있던 `arms spread wide` 가 손 모양에 끼어든 흔적도 보였다 |
| C | 포즈 2, 프롬프트, 시드, 스텝, CFG | 참조모드를 `뼈대` → `원본` 으로 | **양쪽 다 나빠졌다.** 원본 인물의 얼굴·머리색이 닮게 나오고 러닝화까지 딸려 나왔는데, 자세는 원본을 따라가지도 않았다. 양팔이 몸 양옆으로 벌어지고 두 발이 모두 공중에 떠, 원본의 달리기가 아니라 프롬프트의 `spread wide` 쪽으로 갔다 |

## 관찰

* 프롬프트를 통째로 바꿔도 자세는 유지된다. 참조 이미지가 **팔다리를 어디에 둘지**를, 프롬프트가 **무엇을 그릴지**를 정한다. 역할이 갈린다.
* 다만 프롬프트가 자세에 아예 개입하지 않는 것은 아니다. 실험 B 에서 참조 사진의 인물은 주먹을 쥐고 있었는데 결과는 손가락을 펼쳤다. 프롬프트의 `spread wide` 가 새어 들어간 것으로 보인다.
* 결과가 어긋날 때 원인을 두 곳으로 나눠 볼 수 있다. 실험 B 에서 뼈대 그림(`pose_02_skeleton.png`)은 원본의 다리 배치를 정확히 옮겼다. 따라서 좌우가 뒤바뀐 것은 **포즈 추출이 아니라 생성 단계**의 문제다.
* 결함이 늘 같은 자리에 나오지는 않는다. 손가락 오류는 시드를 고정한 채 프롬프트만 바꾸자 사라졌다.
