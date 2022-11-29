## 균형 이진 탐색 트리(Balanced BST)

- 루트 노드로 부터 시작해서 찾고자하는 key 를 각 레벨에 따라 한번의 비교를 통해 key 값을 찾을 수 있다.
- $O(h)$ (h : 높이) 의 시간 복잡도를 가진다.

<br>

insert, delete 연산 모두 search의 영향을 받는다.

<br>

- 이진 트리의 높이
  - $lg(n+1) <= h <= n - 1$

<br>

- 최악의 경우(한 쪽으로 치우친 경우) : $n-1$
- 최선의 경우(완전 이진 트리) : $lg(n+1)$

<br>

균형 이진 탐색 트리는 강제로 이진 탐색 트리의 높이를 $lg(n + 1)$ 로 유지하게 끔 조정을 한다.

- AVL Tree
- RED-BLACK Tree
- (2, 3, 4) - Tree
- splay Tree

<br>

---

## Rotation

- rotation : 높이를 조정하는 작업을 통해 전체 트리의 높이를 조정한다.
- 필요에 따라 left rotation과 right rotation을 수행한다.
- 한 쪽 서브트리의 높이가 다른 쪽 서브트리의 높이보다 크면 높이가 큰 쪽에서 작은 쪽으로 rotation을 수행한다.
- rotation을 여러 번 반복해서 트리의 높이를 맞추는 작업을 수행한다.

<br>

```python
//@self : 균형 이진 트리
//@z : z를 기준으로 rotation을 수행하라
//@self : 균형 이진 트리
//@z : z를 기준으로 rotation을 수행하라

def rotateRight(self, z):
	if not z : return // z가 NIL일 때
	x = z.left

	if not x : return // z의 left node NIL
	b = x.right

	x.parent = z.parent // first link rotation

	// z의 parent가 NIL -> 루트일 때
	// second link rotation
	if z.parent:
			if z.parent.left is z:
						z.parent.left = x
			else:
						z.parent.right = x


	x.right = z // third link rotation
	z.parent = x // fourth link rotation
	z.left = b // fifth link rotation

	// sixth link rotation
	if b: // b가 NIL이 아니면
			b.parent = z

	// 하필 z가 root였다면 root 정보도 update
	if self.root is z:
			self.root = x
```

<br>

<br>

---

## Red-Black Tree

: 가장 유명하고 많이 사용되는 균형 이진 탐색 트리

<br>

![image](https://user-images.githubusercontent.com/67156494/204576819-25a71f4a-5efb-4cf3-990c-36a3a9541003.png)


⭐ 특이하게 leaf node는 NIL이다.

<br>

1. 모든 노드는 red/black으로 구성된다.
2. root 노드 : black
3. leaf 노드 : black
4. red 노드의 자식노드는 무조건 black이다.
5. **각 노드에서 leaf 노드 까지의 black 노드의 수는 모두 동일하다.**

⚠️ 그러나 black 노드의 자식노드가 무조건 red여야하는 건 아니다. → 상관없음!

<br>

- 이진 탐색 트리의 단점 보완 → 트리의 높이가 계속 커지면 검색에 불리해진다.
- 최악의 경우 노드 탐색의 시간 복잡도는 log(n)이 소요될 수 있다.
- 높이는 항상 O(lg n)을 유지해야한다.

<br>

$h(v)$ = v의 높이(height) → leaf 노드 제외

$bh(v)$ = v에서 v를 제외한 leaf node까지의 black 노드의 개수

<br>

- 사실 1 : v 서브 트리의 내부 노드의 개수 ≥ $2^{bh(v)} - 1$
  - 내부 노드의 개수가 적어지면 그만큼 높이는 커진다.
  - 증명 : h(v)에 대한 귀납법

<br>

- 사실 2 : black 노드의 수 ≥ h / 2
  - Ex. r(root)의 서브트리의 내부 노드 개수(= red-black-tree의 노드 개수) ≥ $2^{bh(r)} -1$

<br>

- 증명

  $n >= 2^{\frac h 2}-1$

  $2^{\frac h 2} <= n + 1$

  $h <= 2log_2(n+1)$

  $\therefore h = O(lg n)$

<br>

⭐ red 노드의 자식은 항상 black 노드이다.

- black 노드는 최소한 높이의 1/2 이상이 존재한다.
- 조건 5에 따르면 임의의 노드 v로 부터 leaf 노드까지 블랙 노드의 개수가 일정하므로 균형잡힌 트리로 유지할 수 있다.(한쪽으로 치우치지 않음)

<br>

<br>

## 🔗 Reference

- ****자료구조 - 균형이진탐색트리 - 정의와 회전****
	([https://www.youtube.com/watch?v=Kuw0f3-E-Hw](https://www.youtube.com/watch?v=Kuw0f3-E-Hw))
- ****자료구조 - 균형탐색이진트리 - Red-Black 트리****
	([https://www.youtube.com/watch?v=SHdYv41iCmE&t=1s](https://www.youtube.com/watch?v=SHdYv41iCmE&t=1s))

<br>

<br>

