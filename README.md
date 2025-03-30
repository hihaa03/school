import random

N = 8  # 체스판 크기

def print_board(genes):
    """체스판을 시각적으로 출력하는 함수"""
    board = [['.'] * N for _ in range(N)]
    for row, col in enumerate(genes):
        board[row][col] = 'Q'
    for row in board:
        print(' '.join(row))
    print("\n")

클래스 염색체:
 정의 __init__(자기, 유전자=none):
 유전자가 없는 경우:
 자기.genes = 랜덤.샘플(범위(N), N) # 랜덤으로 유전자(퀸 배치) 생성
 그렇지 않으면:
 자기 자신 genes = 유전자
 자기.fitness = 자기.cal_fitness()

    def cal_fitness(자체):
     """적합도(충돌이 없는 퀸 쌍의 수)를 계산"""
     충돌 = 0
     i의 범위(N):
     범위 (i + 1, N)에서 j에 대해:
     if abs(self.genes[i] - self.genes[j] == abs(i - j):
     갈등 += 1
     반품 28 - 충돌 # 최대 충돌이 없는 경우 28 점

    defutate(자기, mutation_rate=0.1):
     """랜덤한 돌연변이를 적용하여 개체의 다양성을 증가"""
     무작위인 경우. random() < 돌연변이_rate:
     mutation_type = 랜덤.선택(["swap", "시프트", "reverse"])
            
     mutation_type == "swap"인 경우: # 두 개의 퀸 위치 교환
     idx1, idx2 = random.sample(범위(N), 2)
     자기. genes[idx1], 자기. genes[idx2] = 자기. genes[idx2], 자기. genes[idx1]
     엘리프 돌연변이_유형 == "이동": # 마지막 퀸을 랜덤한 위치로 이동
     idx = random.randint(0, N-2)
     셀프.genes.삽입(idx, 셀프.genes.pop ())
     엘리프 돌연변이_유형 == "reverse": # 부분 유전자 구간을 뒤집기
     idx1, idx2 = 정렬됨(random.sample(범위(N)), 2)
                self.genes[idx1:idx2+1] = reversed(self.genes[idx1:idx2+1])
            
            self.fitness = self.cal_fitness()


def tournament_selection(population, k=3):
    """토너먼트 선택 방식: 랜덤하게 k개의 개체를 선택한 후 가장 적합도가 높은 개체 반환"""
    selected = random.sample(population, k)
    return max(selected, key=lambda chromo: chromo.fitness)


def crossover(parent1, parent2):
    """부분적으로 유전자를 교차하여 자식 개체 생성"""
    point = random.randint(1, N - 2)  # 1 ~ N-2 사이에서 랜덤한 교차점 선택
    child_genes = parent1.genes[:point] + [gene for gene in parent2.genes if gene not in parent1.genes[:point]]
    return Chromosome(child_genes)


def genetic_algorithm(pop_size=100, generations=1000):
    """유전 알고리즘을 실행하여 8퀸 문제의 최적 해를 찾음"""
    population = [Chromosome() for _ in range(pop_size)]  # 초기 개체군 생성
    mutation_rate = 0.1  # 초기 돌연변이율
    stagnation_count = 0  # 정체 횟수 (적합도 개선 없음)
    best_fitness = 0

    for generation in range(generations):
        population.sort(key=lambda chromo: chromo.fitness, reverse=True)  # 적합도에 따라 정렬
        
        if population[0].fitness == 28:  # 최적 해(완전한 해) 발견 시 종료
            break
        
        if population[0].fitness > best_fitness:
            best_fitness = population[0].fitness
            stagnation_count = 0  # 적합도가 개선되었으므로 정체 횟수 초기화
     그렇지 않으면:
     정체_카운트 += 1
        
     if stagnation_count > 50: # 50세대 동안 개선이 없으면 돌연변이율 증가
     돌연변이율 = min(0.5, 돌연변이율 + 0.05)
     정체_카운트 = 0
        
     새로운_인구 = 인구 [:10] # 엘리트 보존 (상위 10 개 개체 유지)
        
     len(새로운_인구) < pop_size:
     부모1 = 토너먼트_selection(인구)
     부모2 = 토너먼트_selection(인구)
     자식 = 크로스오버(부모1, 부모2) # 교차 연산 적용
     child.mutate(mutation_rate) # 돌연변이 적용
     새로운_인구.append(아이)
        
     인구 = 새로운_인구 # 새로운 개체군으로 교체

    최고_solution =인구[0]
    인쇄 ("최적 해:", best_solution.genes)
    인쇄 ("적합도:", best_solution.피트니스)
    print_board (best_solution.genes) # 최적 해를 체스판으로 출력
    best_solution 반환

__name__ == "__main__"인 경우:
 유전_algorithm()
\
