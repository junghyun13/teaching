# teaching
# The first line is given the number of words N and the number of words taught K. N is a natural number less than or equal to 50, and K is a natural number less than or equal to 26 or 0. From the second line, N lines are given Antarctic language words, and all Antarctic language words start with "anta" and end with "tica". Assume that the Antarctic language has only N words. Let's find the maximum number of words that students can read! 첫째 줄에 단어의 개수 N과 가르침을 받은 단어의 개수 K가 주어진다. N은 50보다 작거나 같은 자연수이고, K는 26보다 작거나 같은 자연수 또는 0이다. 둘째 줄부터 N개의 줄에 남극 언어의 단어가 주어지고 남극언어의 모든 단어는 "anta"로 시작되고, "tica"로 끝난다. 남극언어에 단어는 N개 밖에 없다고 가정한다. 학생들이 읽을 수 있는 단어의 최댓값을 구해보자!
# c program
#include <stdio.h>
#include <string.h>
int N, K;
char Word[52][17];
int learned[26];
int max;
int count(){ //anta 뒤부터 알파벳 하나씩 배운건지 확인하고, 모두 아는 알파벳이면 ret를 +1 아니면 +0을 하여 N개를 모두 매칭 후에 아는 단어의 수를 return 한다
	int i;
	int ret=0;
	for(i=0;i<N;i++){
		int j=4;
		int know=1;
		for(;;){
			if(Word[i][j]==0x0)
			  break;
			if(!learned[Word[i][j]-'a']){
				know=0;
				break;
			}
			j++;
		}
		if(know)
		  ret++;
		}
		return ret;
}
int sol(int n,int idx){ //안배운 알파벳을 하나씩 배워나가면서 K-5개만큼 모두 배우고 나면 count()를 호출하여 아는 단어의 개수를 세준다.
	int i;
	if(n>=K){
		int cnt=count();
		if(cnt>max)
		  max=cnt;
		return 0;
	}
	for(i=idx;i<26;i++){
		if(!learned[i]){
			learned[i]=1;
			sol(n+1,i);
			learned[i]=0;
		}
	}
	return 0;
}
int main(void){
	int i;
	scanf("%d %d", &N, &K);
	for (i = 0; i < N; i++) {
		scanf("%s", Word[i]);
	}
	//필수적으로 알아야할 단어
	learned['a' - 'a'] = 1;
	learned['n' - 'a'] = 1;
	learned['t' - 'a'] = 1;
	learned['i' - 'a'] = 1;
	learned['c' - 'a'] = 1;
	K -= 5;
	if(K<0){
		printf("%d",0);
		return 0;
	}
	sol(0,0);
	printf("%d",max);
	return 0;
}
# python
N,K=map(int,input().split())
arr=[]
for _ in range(N):
    arr.append(input().strip())
alpha=['a','n','t','i','c']
alpha_l=['b', 'e', 'f', 'g', 'h', 'j', 'k', 'l', 'm','o', 'p', 'q', 'r', 's', 'u', 'v', 'w', 'x', 'y', 'z']
def choose(n,start):
    global result
    if n==0:
        result=max(result,check())
        return 
    for i in range(start,len(alpha_l)):
        alpha.append(alpha_l[i])
        choose(n-1,i+1)
        alpha.pop()
def check():
    result=0
    for words in arr:
        isRead=True
        for i in range(4,len(words)-4): #남극문자에서 앞에 오는 anta 뒤에 오는 문자부터 끝까지
            if words[i] not in alpha:
                isRead=False
                break
        if isRead:
            result+=1
    return result
result=0
if K<5:
    print(result)
else:
    choose(K-5,0)
    print(result)
#input--> 3 6  antarctica antahelloctica antacartica
#result--> 2
