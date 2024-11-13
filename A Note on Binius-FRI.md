# A Note on Binius-FRI

***Content:*** 

# ***Intro:***

This work introduces a *multilinear polynomial commitment scheme* tailored for polynomials over binary tower fields. 

# ***Overview:***

In essence, the polynomial commitment scheme presented in this work ***aligns with the 'Basefold'*** scheme, with the primary distinction being an ***enhanced folding process***, which constitutes the key contribution in my view.

# ***Preliminaries:***

## *1.Notations:*

- Binary field  $K$
- For each $\ell$ $\in \mathbb{N}$, $\mathcal{B}_{\ell}$ is the $\ell$-dimensional boolean hypercube $\{0, 1\}^{\ell} \subset K^{\ell}$
- Integer —> hypercube: $v —> \{v\}: = \sum_{i=0}^{l-1} 2^i v_i$

## *2.Lagrange and Monomial Forms:*

- $K[X_0, X_1,…., X_{\ell -1} ]^{\preceq 1}$ —> the set of multilinear polynomials over $K$ in $\ell$ indeterminates.
- $(1, X_0, X_1, X_0·X_1, …., X_0······X_{\ell -1} )$ —> $K$-basis for $K[X_0, X_1,…., X_{\ell -1} ]^{\preceq 1}$: *multilinear monomial basis in  $\ell$ variables*.
- $2·\ell$ -variate polynomial:
    
     $\widetilde{eq}(X_0, X_1, …, X_{\ell -1}, Y_0, Y_1, …, Y_{\ell -1} ): = \prod_{i=0}^{\ell -1} (1 -X_i) · (1 - Y_i) + X_i · Y_i$$*( \widetilde{eq}(X_0, X_1, …, X_{\ell -1}, v_0, v_1, …, v_{\ell -1} ))_{v \in \mathcal{B}_l}$ yields the $K$-basis of the space $K[X_0, X_1,…,X_{\ell -1}]^{\preceq 1}$* 
    
    *(Since $\widetilde{eq}(X_0, X_1, …, X_{\ell -1}, v_0, v_1, …, v_{\ell -1} ))_{v \in \mathcal{B}_l}$  serves as the Lagrange polynomial in the construction of MLE)*
    
- $\widetilde{mon}(X_0, X_1, …, X_{\ell -1}, Y_0, Y_1, …, Y_{\ell -1} ): = \prod_{i=0}^{\ell -1} 1+X_i(1-Y_i)$
    
    $( \widetilde{mon}(X_0, X_1, …, X_{\ell -1}, v_0, v_1, …, v_{\ell -1} ))_{v \in \mathcal{B}_l}$ yields the multilinear monomial basis in $\ell$ indeterminates. 
    
    *(When $v_i = 0$, the $i$-th element of the product equals 1; when $v_i=1,$ it equals  $x_i$.)*
    

## *3.Error-Correcting Codes:*

### ***1. Definition***

1. A ***code*** of block length $n$ over alphabet $\Sigma$ is a subset of $\Sigma^n$. 
2. $d$ is the *Hamming distanc*e between two vectors.(*the number of components at which they differ*). 
3. A linear $[n, k, d]$ -code over field $K$ is a $k$-dimensional linear space $C \subset K^n$ for which $d(v_0, v_1) \geq d$ holds for each unequal pair of elements $v_0$ and $v_1$ in $C$. 

### ***2. Properties***

1. An $[n,k,d]-code$ $C\subset K^n$ ’s unique ***decoding radius*** is $\lfloor\frac{d-1}{2} \rfloor$:
    
    For each word $u \in K^n$, there exists at most one codeword $v \in C$ such that $d(u, v) < \frac{d}{2}$, and the distance between $u$ and the code $C$ is defined as $d(u,C): = min_{v\in C} d(u,v)$.
    
    *(If there are multiple codewords, say two, that satisfy $d(u, v_i) < \frac{d}{2}$, then by the triangle inequality, $d(v_1, v_2) < d(u, v_1) + d(u, v_2) < d$. This leads to a contradiction.)*
    
2. Given an integer $m \geq 1,$ define ***C’s $m$-fold interleaved code*** as the subset $C^m \subset (K^n)^m \cong (K^m)^n$. 
    - The set $(K^m)^n$ can be viewed as a block code of length $n$ over the alphabet $K^m$, where each element can be represented as a $K^{m \times n}$ matrix, with its rows corresponding to elements of $C$.
    - The distance between two elements in  $C^m$ is defined as the sum of distances between the individual codewords:
        
        Given $v_1=(v_{1,0},v_{1,1},…,v_{1,m-1})$ and $v_2=(v_{2,0},v_{2,1},…,v_{2,m-1})$, where each $v_{1,i}$  and $v_{2,i}$  are codewords in $C$: $d(v_1, v_2) = d(v_{1,0}, v_{2,0}) + d(v_{1,1}, v_{2,1}) + \dots + d(v_{1,m-1}, v_{2,m-1})$
        

### ***3. Reed-Solomon Code***

*(Consider only cases where the message length and block lengths are powers of two.)*

1. ***Parameters***
    - non-negative message length parameter —>  $\ell$
    - non-negative rate parameter —>  $\mathcal{R}$
    - evaluation set $S \subset K$, $|S| = 2^{\ell + \mathcal{R}}$
    - codeword set $C \subset K^{2^{\ell + \mathcal{R}}}$
2. ***R-S code***
    
    Reed-Solomon code $RS_{K, S}[2^{\ell + \mathcal{R}}, 2^{\ell}]$ is defined to be the set $\{(P(x))_{x \in S} | P(X) \in K(X) ^{\preceq 2^{\ell}}\}$, and the distance is $d = n - k + 1 = 2^{\ell + \mathcal{R}} - 2^{\ell} + 1$. 
    
3. ***Berlekamp-Welch algorithm***
    
    *(decoding algorithm for Reed-Solomon within the unique decoding radius)*
    
    ![image.png](A%20Note%20on%20Binius-FRI%208af20a731bc843ee91da3963b90612c8/image.png)
    
    ***proof:*** 
    
    1. If $d(f, C) < \frac{d}{2}$, the algorithm necessarily returns unique polynomial $P(x)$ of degree less than $2^{\ell}$:
        
        $d(f, C) < \frac{d}{2}$ —> There are ***at most*** $\lfloor \frac{d-1}{2} \rfloor$ positions where $f(x)|_{x \in S}$ differs from a codeword in $C$.
        
        $P(X)  = -B(X) | A(X)$ —> $Q(X, Y) = A(X) (Y - P(X))$;
        
        $Q(x, f(x)) = 0$ —> $A(x) (f(x) - P(x)) = 0$ —> $f(x) = P(x)$ at $|S| -  \lfloor \frac{d-1}{2} \rfloor$ points, where $A(x) = 0$ at $\lfloor \frac{d-1}{2} \rfloor$ points, which implies that $A(x)$ is of degree $\lfloor \frac{d-1}{2} \rfloor$.
        
    2. Otherwise, the algorithm returns $\perp$:
        
        $d(f, C) \geq \frac{d}{2}$ —> There are ***at least*** $\frac{d}{2}$ positions where $f(x)|_{x \in S}$ differs from a codeword in $C$. —> $f(x) = P(x)$ holds at ***fewer*** than $|S| - \frac{d}{2}$ positions. —> $A(x)$ has to be 0 at more than $\frac{d}{2}$ positions. However, since the degree of $A(x)$ is only $\lfloor \frac{d-1}{2} \rfloor$, a contradiction arises. 
        

## *4.Novel Polynomial Basis:*

### ***1. subspace polynomial***

1. ***Subspace vanishing Polynomials:***
    
    ***Definition:*** 
    
    Let $F_{q^m}$ be an extension finite field with dimension $m$ over $F_q$. The elements in $F_{q^m}$ are represented as $\{\omega_i\}_{i=0}^{q^m -1}$ in order. Assume that $V$ be the $m$-dimensional vector space spanned by $(v_0, v_1,.., v_{m-1})$ over $F_q$. Then each element $\omega_i$ can be expressed as:
    
    $\omega_i = c_0 v_0 + c_1 v_1 +… + c_{m-1}v_{m-1}$ , where $c_i \in F_q$. 
    
    The subspace vanishing polynomial is defined as:
    
    $$
    W_j(X) = \prod_{i=0}^{q^j -1} (X - \omega_i)
    $$
    
    ***Properties:***
    
    1. $W_j(X)$ has the form:
        
        $$
        \color{orange}W_j(X) = \sum_{i=0}^j \alpha_j X^{q^j}, \alpha_j \in F_{q^m}
        $$
        
        ***Proof:***
        
        Proof by Induction:
        
        $j = 0$ —> $W_0(X) = (X - \omega_0 )$
        
        Since $\omega_0 = 0$, $W_0(X) = X$. —> holds. 
        
        Assume that $W_j(X) = \sum_{i=0}^j \alpha_j X^{q^j}$ holds, then
        
        $$
        \color{orange}W_{j+1} (X)= W_j(X) (X - \omega_{q^j}) ( X - \omega_{q^j +1})… (X - \omega_{q^{j+1}-1})
        $$
        
        Since $\omega_{q^j + i}$ for $i \in F_q$ is expressed as $\sum_{k=0}^{j} c_k v_k$, and $\sum_{k=0}^{j-1} c_k v_k$ forms the set $\{\omega\}_j = (\omega_0, \ldots, \omega_{q^j - 1})$, $\omega_{q^j + i}$ can be written as $(\{\omega\}_j + c_j v_j)$ for $c_j \in F_q$. Therefore, 
        
        $$
        \color{orange}\begin{align}W_{j+1}(X) &= \prod_{c_j=0}^{q-1}W_j(X - c_jv_j)\\&=  (\prod_{c_j=0}^{q-1}(\sum_{i=0}^j \alpha_j (X-c_jv_j)^{q^j}))\\&= (\prod_{c_j=0}^{q-1}(\sum_{i=0}^j\alpha_j X^{q^j} -\alpha_j(c_jv_j)^{q^j})) \\&= \prod_{c_j=0}^{q-1}W_j(X) + c_jK_j\end{align} 
        $$
        
        where $K_j$ is a constant and equal to $\sum_{i=0}^j\alpha_j  (v_j)^{q^j} = W_j(v_j)$. 
        
        As $W_j(X)^k = \sum_{i=0}^j \alpha^k X^{k q^i}$, it can be implied that $W_{j+1}(X) = \sum_{i=0}^{j+1} \beta_j X^{q^i}$
        
2. Subspace polynomials are a special class of ***linearized polynomials,*** such that:

$$
\forall x, y \in F_{q^m}, W(x + y) = W(x) + W(y)
$$

### ***2. Novel Polynomial Basis***

1. ***Regular Polynomial Basis***
    
    The polynomial basis of $K[X] ^{\preceq 2^{\ell}}$ is $(1, X, X^2, …, X^{2^{\ell} -1 } )$, which is consisted of monomial polynomials. 
    
2. ***Alternate Polynomial Basis***
    
    ***a.Definition:***
    
    Define a new polynomial basis of the form: $(X_0(X), X_1(X),…, X_{2^{\ell}-1}(X) )$, and each $X_i(X)$ is defined as:
    
    $$
    X_i(X) = \prod_{j=0}^{r-1} (\frac{W_j(X)} {W_j(\omega_{2^j}) } )^{i_j}
    $$
    
     where  $i$ is written in binary representation as:
    
    $$
    i = i_0 + i_1 · 2 + … + i_{r-1} · 2^{r-1}. \forall i_j \in \{0, 1\}
    $$
    
            and  $W_j(X)$ is the subspace vanishing polynomial for $F_{2^j}$. 
    
    ***b.Properties:***
    
    1. It can be seen that if i_j =0, then $(\frac{W_j(X)} {W_j(\omega_{2^j}) } )^{i_j} = 1$, and $deg(X_i) = \sum_{j=0}^{r-1} i_j \times deg(W_j(X))  = \sum_{j=0}^{r-1} i_j \times 2^j = i$. 

## *5.FRI:*

FRI yields an ***IOP pf Proximity*** for the Reed-Solomon code $RS_{K, S}[2^{\ell + \mathcal{R}}, 2^{\ell}]$:

1. The rate of $RS_{K, S}[2^{\ell + \mathcal{R}}, 2^{\ell}]$ is $\frac{2^{\ell}}{2^{\ell + \mathcal{R}}} = \frac{1}{2^{\mathcal{R}}}$, which is short as $\rho: = 2^{- \mathcal{R}}$. 
2. FRI’s verifier complexity is polylogarithmic in $2^{\ell}$ for checking the claim whereby some oracle $[f$] which represents a function $f: S \longrightarrow  K$ is close to a codeword $(P(x))_{x \in S}$. 

## *6.The Tower Algebra:*

### ***1. Wiedermann’s tower:***

Wiedemann’s tower refers to a method for constructing binary towers, which are continuous extensions of binary fields. The towers are built as follows:

- ***Initial Phase:***
    
    $\tau_0 = F_2 = \{0, 1\}$
    
- ***Phase 1:***
    - ***Step 1**: Introduce an imaginary element ‘$x_1$’, declare that $x_1^2 = x_1 + 1$.*
    - ***Step 2:** Construct $\tau_1= F_2[x_1] / (x_1^2 + x_1 + 1) = \{a_0 + b_0 x_1|a_0,b_0 \in \tau_0\}$*
        
                                                                                                                        *—> $\tau_1 = F_{2^{2\times 1}} = F_{2^2}$*
        
- ***Phase 2:***
    - ***Step 1**: Introduce an imaginary element ‘$x_2$’, declare that $x_2^2 = x_1\times x_2 + 1$.*
    - ***Step 2:** Construct $\tau_1= F_2[x_1,x_2] / (x_2^2 + x_1*x_2 + 1) = \{a_1 + b_1 x_2)| a_1, b_1 \in \tau_1\}$*
    
                                                                                                                          *—> $\tau_2 = F_{2^{2\times 2}} = F_{2^4}$*
    
- …
- ***Phase $\iota$:***
    - ***Step 1**: Introduce an imaginary element ‘$x_\iota$’, declare that $x_{\iota}^2 = x_{\iota}\times x_{\iota-1} + 1$.*
    - ***Step 2:** Construct $\tau_{\iota}= F_2[x_1,x_2,...,x_{\iota}] / (x_{\iota}^2 + x_{\iota}*x_{\iota-1} + 1) = \{a_{\iota-1} + b_{\iota-1} x_{\iota})| a_{\iota-1}, b_{\iota-1} \in \tau_{\iota-1}\}$*
    
                                                                                                                           *—> $\tau_{\iota} = F_{2^{2\times {\iota}}}$*
    
1. It can be seen that $\tau_1 \subset \tau_2…\subset\tau_\iota$.
2.  The monomial $F_2$-basis of the binary tower $\tau_\iota$ is $({\beta_v})_{v\in \mathcal{B}_\iota}$:= $( \widetilde{mon}(X_0, X_1, …, X_{\iota -1}, v_0, v_1, …, v_{\iota -1} )){v \in \mathcal{B}\iota}$;

### ***2. Tower Algebra:***

‘Tower Algebra’ is a mathematical data structure which is used in the ‘packing’ technique.

- Informally, it's a technique that groups multiple elements into one unit to perform computations over a large space more efficiently.
- More specifically, as for the field $\tau_{\iota+\kappa}$, we could group $\kappa$ elements into a vector and treat field $\tau_{\iota+\kappa}$ as a $\tau_\iota$ field for the ($a_v)_{v\in \mathcal{B_{\kappa}}}$ element. Then, the multiplication operation on $\tau_{\iota + \kappa}$  will no longer be restricted to binary element of 0 and 1. Instead, it will operate 'independently' over larger fields, working with $2^{\kappa}$ elements.
- The formal definition is described below. We fix parameters $\iota, \kappa$ and $\tau$ in $N$, $\tau \geq \iota$. $\tau_\iota$ represents the coefficient field, $\kappa$ is the packing parameter. $\tau_\tau$ yields a cryptographically sized extension of $\tau_\iota$:

# ***Binius IOPCS over Large Fields:***

## *1. **Construction of Continuous Subspaces***

### ***1. Subspace construction:***

Initialize $S^{(0)}:= <\beta_0, \beta_1,…, \beta_{\ell + \mathcal{R} -1} >$. For each $i \in (0, …, \ell -1)$, set:

$$
q^{(i)} = \frac{W_i(\beta_i)^2} { W_{i+1}(\beta_{i+1}) } X · (X+1)
$$

Besides, let $S^{(i+1)} := im(q^{(i)} |_{ S^{(i)}}) (i.e: S^{(i+1)} := q^{(i)} (x) |_{x\in S^{(i)}})$ 

The following lemma is derived from this construction.

### ***2. Properties of subspace:***

***1.Lemma:***

For each $i \in \{0, …, \ell -1\}, ker(q^{(i)}) \subset S^{(i)}$. （ker stands for kernel: $ker(q^{(i)} = \{x| q^{(i)}(x) = 0\}$)

***Proof:***

It can be seen that $ker(q^{(i)}) = \{0, 1\}$ for each $i \in \{0, …, \ell -1\}$, the claim then reduces to proving that $\{0, 1\} \subset S^{(i)}$. 

If it can be proved that $S^{(i)} == im(\hat{W}i(X) |_{S^{(0)}})$, then the claim holds. 

Define $\hat{W_i}(X)$  as $\frac{W_i(X)}{W_i(\beta_i)}$, so that $\hat{W}_i(X) = 0$ for all $\beta_j$ with  $j < i$, and $\hat{W}_i(\beta_i) = 1$, thus $ker(q^{(i)}) \subset im(\hat{W}_i(X) |_{S^{(0)}})$

Since $S^{(i)} := im(q^{(i-1)} |_{ S^{(i-1)}})$, the claim can be further reduced to prove  $im(q^{(i-1)} \circ \hat{W}_{i-1} |_{ S^{(0)}}) == im(\hat{W}_i(X) |_{S^{(0)}})$:

$$
\color{orange}\begin{align}(q^{(i-1)} \circ \hat{W}_{i-1})(X) &= \frac{W_{i-1}(\beta_{i-1})^2} { W_{i}(\beta_{i}) } \frac{W_{i-1}(X)}{W_{i-1}(\beta_{i-1})} · (\frac{W_{i-1}(X)}{W_{i-1}(\beta_{i-1})}+1)\\ &=\frac{W_{i-1}(\beta_{i-1})^2} { W_{i}(\beta_{i}) } \frac{W_{i-1}(X)}{W_{i-1}(\beta_{i-1})} · (\frac{W_{i-1}(X)+W_{i-1}(\beta_{i-1})}{W_{i-1}(\beta_{i-1})})\\&=\frac{W_{i-1}(X) · (W_{i-1}(X)+W_{i-1}(\beta_{i-1}))} { W_{i}(\beta_{i}) }\\&= \frac{W_{i}(X) } { W_{i}(\beta_{i}) } = \hat{W}_{i}(X)\end{align}
$$

(The third step is derived of the definition of $W(X)$)

This Lemma yields some corollaries as shown below:

***Corollary 1:*** 

For each  $i \in \{0, …, \ell\}, S^{(i)} = im(\hat{W}_i(X) |_{S^{(0)}}).$ 

(This is explicitly proven in the preceding content.)

***Corollary 2:*** 

For each  $i \in \{0, …, \ell\}, \hat{W}_i = q^{(i-1)} \circ \cdots \circ q^{(0)}$ . 

***Proof:*** 

Since $(q^{(i-1)} \circ \hat{W}_{i-1})(X) = \hat{W}_{i}(X)$, by induction, the claim can be proven. 

***Corollary 3:*** 

For each  $i \in \{0, …, \ell\},$  the set $(\hat{W}_i(\beta_i), \hat{W}_i(\beta_{i+1}),…,  \hat{W}_i(\beta_{\ell+ \mathcal{R}- 1})）$ is an $F_2$ basis of the space $S$$^{(i)}$. 

Since $S^{(i)} = im(\hat{W}i(X) |_{S^{(0)}})$, then dimension of $S^{(i)}$ is $\ell+ \mathcal{R} - i$. 

Besides, the subspace $V_i:= <\beta_i, …, \beta_{\ell+ \mathcal{R}- 1}>$ clearly satisfies that $V_i \subset  S^{(0)}$, so that $\hat{W}_i(X) |_{V_i} \subset \hat{W}_i(X) |_{S^{(0)}} = S^{(i)}$

Finally, as $\hat{W}_i(X$)’s kernel is $(\beta_0, \beta_1, …, \beta_{i-1})$, $(\hat{W}_i(\beta_i), \hat{W}_i(\beta_{i+1}),…,  \hat{W}_i(\beta_{\ell+ \mathcal{R}- 1})$ does not intersect with each other. 

***Remark:*** 

Based on Corollary 3, it is convenient to map $S^{(i)}$ to $S^{(i+1)}$ by projecting away its 0-th indexed component and changing the basis from $<\hat{W}_i(\beta_i), \hat{W}_i(\beta_{i+1}),…,  \hat{W}_i(\beta_{\ell+ \mathcal{R}- 1})>$ to $<0, \hat{W}_{i+1}(\beta_{i+1}),…,  \hat{W}_{i+1}(\beta_{\ell+ \mathcal{R}- 1})>$. And for each $i \in \{0, …, \ell -1 \}$, and $y \in S^{(i+1)}$, the two elements $x_1$ and $x_2$ corresponding to the same $y$ (i.e: $q^{(i)}(x_1) = q^{(i)}(x_2) = y$) differ only at the $0$-th components, while matching the coordinate representation of $y$ in all other components.   

***Proof:*** 

Express each element $x \in$ $S^{(i)}$ in coordinates with respect to basis $<\hat{W}_i(\beta_i), \hat{W}_i(\beta_{i+1}),…,  \hat{W}_i(\beta_{\ell+ \mathcal{R}- 1})>$:

$$
\color{orange}x = c_0\hat{W}_i(\beta_i) + c_1\hat{W}_i(\beta_{i+1}) +… + c_{\ell+ \mathcal{R} - i}\hat{W}_i(\beta_{\ell+ \mathcal{R}- 1}), c_i \in \{0, 1\}.
$$

then 

$$
\color{orange}\begin{align}q^{(i)}(x) &= \frac{W_i(\beta_i)^2} { W_{i+1}(\beta_{i+1}) } x · (x+1)\\&=\frac{W_i(\beta_i)^2} { W_{i+1}(\beta_{i+1}) }(c_0\hat{W}_i(\beta_i) +… + c_{\ell+ \mathcal{R} - i}\hat{W}_i(\beta_{\ell+ \mathcal{R}- 1})\\& \quad (c_0\hat{W}_i(\beta_i) +… + c_{\ell+ \mathcal{R} - i}\hat{W}_i(\beta_{\ell+ \mathcal{R}- 1}+1)\\&=\frac{W_i(\beta_i)^2} { W_{i+1}(\beta_{i+1}) }(c_0(\hat{W}_i(\beta_i)^2+\hat{W}_i(\beta_i))+\\&\quad...+c_{\ell+ \mathcal{R} - i}(\hat{W}_i(\beta_{\ell+ \mathcal{R}- 1})^2+\hat{W}_i(\beta_{\ell+ \mathcal{R}- 1})))\\&=c_0 0 + c_1\hat{W}_{i+1}(\beta_{i+1})+...+c_{\ell+ \mathcal{R} - i}\hat{W}_{i+1}(\beta_{\ell+ \mathcal{R}- 1})\end{align}
$$

The transition from the third equation to the fourth holds because:

$$
\color{orange}\hat{W}_i(x)^2 + \hat{W}_i(x) = \frac{W_i(x)^2 + W_i(x)W_i(\beta_i)}{W_i(\beta_i)^2} =\frac{W_{i+1}(x)}{W_i(\beta_i)^2}
$$

***2.Properties:***

1. If the dimension of the initial space is $\ell$, and the initial space basis are $<\beta_0, \beta_1,…, \beta_{\ell-1}>$, which is actually $<\hat{W}_0(\beta_0), \hat{W}_0(\beta_1),…, \hat{W}_0(\beta_{\ell-1})>$ since $\hat{W}_0(X) = X$, then the dimension of the $i$-th subspace would be $\ell - i,$ and the normalized subspace vanishing polynomial for the $i$-th space can be expressed as: $\hat{W}_0^{(i)}, \hat{W}_1^{(i)}, …, \hat{W}_{\ell-i-1}^{(i)}$, which are equal to $X, q^{(i)}, q^{(i+1)} \circ q^{(i)}, …, q^{(\ell-2)} \circ…\circ q^{(i)}$, respectively. 

***Proof:***

This can be proven by induction.

The basis of $i$-th subspace is $<\hat{W}_i(\beta_i), \hat{W}_i(\beta_{i+1}),…,  \hat{W}_i(\beta_{\ell- 1})>$ according to ***Corollary 3.*** We regard this basis as $(\rho_0, \rho_1,.., \rho_{l-i-1})$ for short.

$\hat{W}_0^{(i)} = \frac{W_0^{(i)}(X)}{W_0^{(i)}(\rho_0)} = \frac{X}{1} = X$

 $\hat{W}_1^{(i)} = \frac{W_1^{(i)}(X)}{W_1^{(i)}(\rho_1)} = \frac{X(X+\rho_0)}{\hat{W}_i(\beta_{i+1})(\hat{W}_i(\beta_{i+1})+\rho_0)} = \frac{W_i(\beta_i)^2}{W_{i+1}(\beta_{i+1})}(X(X+1)) = q^{(i)}$

Assume that for $1<k\leq{\ell - i -1}, \hat{W}_k^{(i)} = q^{(i+k-1)}\circ...\circ q^{(i)}$. 

$\hat{W}_{k+1}^{(i)} = \frac{W_{k+1}^{(i)}(X)}{ W_{k+1}^{(i)}(\rho_{k+1})}=\frac{W_k^{(i)}(X)(W^{(i)}_k(X)+W^{(i)}_k(\rho_k))}{W_{k}^{(i)}(\rho_{k+1})(W_{k}^{(i)}(\rho_{k+1})+W_{k}^{(i)}(\rho_{k}))} = \frac{\hat{W}_k^{(i)}(X)(\hat{W}^{(i)}_k(X)+1)}{\hat{W}_{k}^{(i)}(\rho_{k+1})(\hat{W}_{k}^{(i)}(\rho_{k+1})+1)}$

$\hat{W}^{(i)}_k(\rho_{k+1}) = \hat{W}^{(i)}_k(\hat{W}_i(\beta_{i+k+1})=(q^{(i+k-1)}\circ...\circ q^{(i)}\circ\hat{W}_i)(\beta_{i+k+1}) = \hat{W}_{i+k}(\beta_{i+k+1})$ 

  *(According to the above lemma)*

Thus, $\hat{W}_{k+1}^{(i)} = \frac{\hat{W}_k^{(i)}(X)(\hat{W}^{(i)}_k(X)+1)}{\hat{W}_{k}^{(i)}(\rho_{k+1})(\hat{W}_{k}^{(i)}(\rho_{k+1})+1)}= \frac{\hat{W}_k^{(i)}(X)(\hat{W}^{(i)}_k(X)+1)}{\hat{W}_{i+k}(\beta_{i+k+1})(\hat{W}_{i+k}(\beta_{i+k+1}+1)}=\frac{W^2_{i+k}(\beta_{i+k})(\hat{W}_k^{(i)}(X)(\hat{W}^{(i)}_k(X)+1))}{W_{i+k+1}(\beta_{i+k+1})} = q^{(i+k)}\circ \hat{W}_k^{(i)}=q^{(i+k)}\circ q^{(i+k-1)}\circ...\circ q^{(i)}$

1. For the $i$-th subspace, the novel polynomial basis is defined as $X_j^{(i)} = \prod_{k=0}^{l-i} (\hat{W}_k^{(i)})^{j_k}$ for $j \in \{0, …, 2^{l-i}-1\}$ and $j_k$ is the $k$-th bit of $j$. This novel polynomial basis has the following property:
    
    $$
    X_{2j}^{(i)} (X) = X_j^{(i+1)}(q^{(i)}(X)); X_{2j+1}^{(i)} (X) = X\cdot X_j^{(i+1)}(q^{(i)}(X))
    $$
    
    for $j \in \{0, …, 2^{l-i-1}-1\}$
    
    ***Proof***
    
    $j = j_0 + j_1*2 + … + j_{l-i-2}*2^{l-i-2}$ 
    
    Then $2j =  0 +  j_0*2 + j_1*2^2 + … + j_{l-i-2}*2^{l-i-1}$, $2j+1 =  1 +  j_02 + j_12^2 + … + j_{l-i-2}*2^{l-i-1}$
    
    $X_{2j}^{(i)}(X) = (\hat{W}_0^{(i)}(X))^0(\hat{W}_1^{(i)}(X))^{j_0}..(\hat{W}_{l-i-1}^{(i)}(X))^{j_{l-i-2}}=(q^{(i)})^{j_0}(q^{(i+1)} \circ q^{(i)})^{j_1}...(q^{(\ell-2)} \circ…\circ q^{(i)})^{j_{l-i-2}}$
    
    $X_{2j+1}^{(i)}(X) = (\hat{W}_0^{(i)}(X))^1(\hat{W}_1^{(i)}(X))^{j_0}..(\hat{W}_{l-i-1}^{(i)}(X))^{j_{l-i-2}}=X(q^{(i)})^{j_0}(q^{(i+1)} \circ q^{(i)})^{j_1}...(q^{(\ell-2)} \circ…\circ q^{(i)})^{j_{l-i-2}}$
    
    $X_{j}^{(i+1)}(X) = (\hat{W}_0^{(i+1)}(X))^{j_0}(\hat{W}_1^{(i+1)}(X))^{j_1}..(\hat{W}_{l-i-2}^{(i+1)}(X))^{j_{l-i-2}} = X^{j_0}(q^{i+1})^{j_1}...(q^{(\ell-2)} \circ…\circ q^{(i+1)})^{j_{l-i-2}}$ 
    
    *(According to property 1)*
    
    Thus, $X_{2j}^{(i)} (X) = X_j^{(i+1)}(q^{(i)}(X))$, $X_{2j+1}^{(i)} (X) = X\cdot X_j^{(i+1)}(q^{(i)}(X))$
    

## *2. Enhanced Folding Mechanism**

Here gives the general folding process applicable to continuous space.

***Definition 1:*** 

For $i \in \{0, …, \ell -1\}$ and a map $f^{(i)}: S^{(i)} \longrightarrow  L$. For each $r \in L$, define the map $fold(f^{(i)}, r)：S^{(i+1)} \longrightarrow  L$ as follows:

$$
\color{red}fold(f^{(i)}, r): \forall y \in S^{(i+1)} ,  y = \begin{bmatrix}
1-r  & r
\end{bmatrix}\begin{bmatrix}
x_1 &-x_0 \\
-1 &1
\end{bmatrix}\begin{bmatrix}
f^{(i)}(x_0)\\
f^{(i)}(x_1)
\end{bmatrix}
$$

where $(x_0, x_1) = q^{{(i)}^{-1}}(y)$

***Remark 1:***

The folding process applied in multiplicative FRI is a specialized case with $x_1 = -x_0$ . 

***Definition 2:*** 

Fix a positive folding factor $\vartheta$, for $i \in \{0, …, \ell -\vartheta\}$ and a map $f^{(i)}: S^{(i)} \longrightarrow  L$. For each tuple $(r_0, r_1,.., r_{\vartheta-1}) \in L^\vartheta$, define the map $fold(f^{(i)}, r_0,...,r_{\vartheta-1})：= fold(...(fold(f^{(i)}, r_0),...,r_{\vartheta-1})$ 

According to definition 1 and 2, we have the following lemma:

***Lemma:***

For positive folding factor $\vartheta$ and index $i \in \{0,…, \ell - \vartheta\}$, $y \in S^{(i + \vartheta)}$, there exists a $2^\vartheta \times 2^\vartheta$ invertible matrix $M_y$ with entries in $K$, such that, for each map $f^{(i)}: S^{(i)} \longrightarrow  L$  and each tuple $(r_0, …, r_{\vartheta-1}) \in L^\vartheta$ of folding challenges, we have the matrix representation:

$$
\color{orange}\begin{align}&fold(f^{(i)}, r_0,...,r_{\vartheta-1})(y) \\&= \left [ \otimes _{j=0}^{\vartheta -1}(1-r_j, r_j) \right ] \cdot \begin{bmatrix}
&  & \\
& M_y & \\
&  &
\end{bmatrix} \cdot \begin{bmatrix}
f^{(i)}(x_0) \\
... \\f^{(i)}(x_{2^\vartheta -1})\end{bmatrix}\end{align}
$$

where $(x_0, x_1,…,x_{2^\vartheta -1}) = (q^{(i+\vartheta -1)}\circ ...\circ q^{(i)} )^{-1}(y)$.

***Proof:***

This can be proven by induction.

*Case 1:*

$\vartheta = 1$: The claim is equivalent to Definition 1.. And the matrix $\begin{bmatrix}
f^{(i)}(x_0)\\
f^{(i)}(x_1)
\end{bmatrix}$ is invertible since its determinant is $x_1 -x_0$ , which is not zero. 

Suppose that the claim holds for $\vartheta -1$. Let $(z_0, z_1) = q^{(i+\vartheta -1)}(y)$, as well as $(x_0, x_1,…,x^{2^\vartheta - 1}) = (q^{(i+\vartheta -1)}\circ ...\circ q^{(i)} )^{-1}(y)$, then we have $(x_0, x_1,…,x^{2^\vartheta - 1}) = (q^{(i+\vartheta -2)}\circ ...\circ q^{(i)} )^{-1}(z_0, z_1)$, thus

$$
\color{orange}\begin{align}&\quad fold(f^{(i)}, r_0,...,r_{\vartheta-1})(y) \\&= [ 1-r_{\vartheta-1} ,r_{\vartheta-1}] \cdot \begin{bmatrix}
z_1& -z_0  \\
-1 &1 \\
\end{bmatrix} \cdot \begin{bmatrix}
 fold(f^{(i)}, r_0,...,r_{\vartheta-2}(z_0)
 \\fold(f^{(i)}, r_0,...,r_{\vartheta-2}(z_1)\end{bmatrix}\\&=[ 1-r_{\vartheta-1} ,r_{\vartheta-1}] \cdot \begin{bmatrix}
z_1& -z_0  \\
-1 &1 \\
\end{bmatrix}\cdot \begin{bmatrix}
 \left [ \otimes _{j=0}^{\vartheta -2}(1-r_j, r_j) \right ] & \\
  &\left [ \otimes _{j=0}^{\vartheta -2}(1-r_j, r_j) \right ]
\end{bmatrix}\\& \quad\cdot \begin{bmatrix}
 M_{z_0} & \\
  &M_{z_1}
\end{bmatrix} \cdot\begin{bmatrix}
 f^{(i)}(x_0)\\...
 \\f^{(i)}(x_{2^{\vartheta -1}})

\end{bmatrix} \\&=[ 1-r_{\vartheta-1} ,r_{\vartheta-1}]\begin{bmatrix}
 \left [ \otimes _{j=0}^{\vartheta -2}(1-r_j, r_j) \right ] & \\
  &\left [ \otimes _{j=0}^{\vartheta -2}(1-r_j, r_j) \right ]
\end{bmatrix} \cdot \\&\quad\begin{bmatrix}
 diag(z_1) &  diag(-z_0)\\
 diag(-1)  &diag(1)
\end{bmatrix}\cdot \begin{bmatrix}
 M_{z_0} & \\
  &M_{z_1}
\end{bmatrix} \cdot\begin{bmatrix}
 f^{(i)}(x_0)\\...
 \\f^{(i)}(x_{2^{\vartheta -1}})

\end{bmatrix} \end{align}
$$

where $diag(z_i)$ means constructing a diagonal matrix with element $z_i$ placed on the main diagonal and all other elements set to zero. 

It can be seen that the product of the first two matrices equals $\left [ \otimes _{j=0}^{\vartheta -1}(1-r_j, r_j) \right ]$, and the product of the middle two  $2^\vartheta \times 2^\vartheta$ matrices is invertible, which corresponding to $M_y.$ 

***Remark 2:***

The matrix $M_y$ depends only on the value of $y \in S^{(i+\vartheta)}$;

## *3. IO-PCS over Large Field*

The simple version of the polynomial commitment scheme is shown as follows:

### ***1.Setup Phase:***

- $L = F_{2^r}, r \geq \ell + \mathcal{R}$, the regular basis of $L$ is  $(\beta_0, \beta_1, .., \beta_{r-1})$
- the L-basis for  $L[x]^{\prec 2^\ell}$ is: $(X_0(X), X_1(X), …, X_{2^\ell-1}(X))$
- folding factor  $\vartheta$
- repetition parameter $\gamma$
- domains $S^{(0)}, …, S^{(\ell)}$
- polynomials $q^{(0)}, …, q^{(\ell-1)}$

### ***2.Commit Phase:***

Private Input: $t(X_0, X_1, …,, X_{\ell -1}) \in K[X_0, X_1,…, X_{\ell-1}]^{\preceq 1}$

***Step 1:*** P computes $\{t(v)\}_{v \in \mathcal{B}_{\ell}}$

***Step 2:*** P construct the *univariate* polynomial $P(x) = \sum_{v \in \mathcal{B}_{\ell}} t(v) X_{\{v\}}(X)$ 

***Step 3:*** P computes the R-S codeword $f: S^{(0)} \longrightarrow  L$ defined by $f: x \longrightarrow  P(x)$

***Step 4:*** Commits to $f$ as $[f]$

### ***3.Evaluation Phase:***

***Claim***: P claims that $t(\vec{r}) = s$.

***Public Input***: $[f], s \in L$, and $\vec{r} \in L^{\ell}$

***Private Input:*** $t(X_0, X_1, …,, X_{\ell -1}) \in K[X_0, X_1,…, X_{\ell-1}]^{\preceq 1}$

1. P construct $h(X_0, X_1, …,, X_{\ell -1}) = t(X_0, X_1, …,, X_{\ell -1})\widetilde{eq}(r_0,r_1,...r_{\ell -1},X_0, X_1,...,X_{\ell-1})$
2. Set $f^{(0)} = f$, $s_0 = s$.
    
    for $i \in \{0, …, \ell -1\}$ do:
    
    1. ***P —> V***: the univariate polynomial $h_i(x) = \sum_{v\in \mathcal{B}_{\ell-i-1}}h(r_0’,…,r_{i-1}’, X, v_0, v_1,…,v_{\ell -i-2})$
    2. V checks $s_i ?= h_i(0) + h_i(1)$, samples $r_i’,$ and set $h_i(r_i’) = s_{i+1}$
    3. ***V —> P***: $r_i’$
    4. P defines $f^{(i+1)} = fold(f^{(i)}, r_i’)$
    5. if $i+1 = \ell$: $P —> V: c = f^{(\ell)}(0,..0)$ *(R zeros)*
    6. else if $\vartheta | i+1$ P commits to $f^{(i+1)}$, and send $[f^{(i+1)}]$ to V
3. V checks $s_\ell ?= c \cdot\widetilde{eq}(r_0,r_1,...r_{\ell -1},r_0’,…,r_{\ell-1}’)$
4. V executes the following querying procedure: 
    
    for $\gamma$ repetitions do (*The purpose of repeatition is to achieve a level of security*）
    
    1. V samples v $\in \mathcal{B}_{\ell + \mathcal{R}}$
    2. for $i \in \{0, \vartheta, …, \ell - \vartheta\}$ do:
        1. for each $u \in \mathcal{B}_\vartheta$, V queries $[f^{(i)}](u_0, u_1,…,u_{\vartheta-1}, v_{i+\vartheta}, …, v_{\ell+\mathcal{R}-1})$
        2. if $i > 0$, V additionally checks $c_i ?= f^{(i)}(v_i, …, v_{\ell + \mathcal{R}-1})$
        3. V defines $c_{i+\vartheta}:=fold(f^{(i)}, r_i’, …, r_{i+\vartheta-1})(v_{i+\vartheta}, …, v_{\ell + \mathcal{R} -1})$
    3. V checks $c_\ell ? = c$

### ***4.Completeness Proof:***

It can be seen that $t(\vec{r}) = \sum_{v \in \mathcal{B}_\ell}h(v)=s$, so the first several step is actually a sum-check on $h.$ By the completeness of sum-check, an honest prover will always pass on $s_i = h_i(0)+h_i(1)$, and this process will reduce to check $s_\ell ?= h(r_0’, r_1’,…, r_{\ell-1}’) ? = t(r_0’, r_1’,…, r_{\ell-1}’) \widetilde{eq}(r_0, …, r_{\ell-1}, r_0’, r_1’,…, r_{\ell-1}’)$.

Honest prover will first generate an univariate polynomial $P(X)$ and compute $f = \{P(x) | x \in S^{(0)}\}$. The ‘***code***’ over the $i$-th space is denoted as $f^{(i)}$, which is assumed to be the evaluation over $S^{(i)}$ of the polynomial $P^{(i)}(X) = \sum_{j=0}^{2^{\ell-i}-1}a_jX_j^{(i)}(X)$. It can be proved that $P^{(i+1)}(X) = \sum_{j=0}^{2^{\ell-i-1}-1}((1-r_i’)a_{2j} + r_i'a_{2j+1})X_j^{(i+1)}(X)$:

***Sub-Proof:***

The even component of $P^{(i)}(X)$ are $a_{2j}X_{2j}^{(i)}(X)$ and the odd ones are $a_{2j+1}X_{2j+1}^{(i)}(X)$, which equal to $a_{2j}X_{j}^{(i+1)}(q^{(i)}(X))$  and $X\cdot a_{2j+1}X_{j}^{(i+1)}(q^{(i)}(X))$, respectively.  

 Let $P_0^{(i+1)}(X) = \sum_{j=0}^{2^{\ell-i-1}-1}a_{2j}X_j^{(i+1)}(X)$ and $P_1^{(i+1)}(X) = \sum_{j=0}^{2^{\ell-i-1}-1}a_{2j+1}X_j^{(i+1)}(X)$ , then the $P^{i}(X)$ can be written as:

$$
\color{orange}P^{(i)}(X) = P_0^{(i+1)}(q^{(i)}(X))+XP_1^{(i+1)}(q^{(i)}(X))
$$

 let $(x_0, x_1) = q^{(i)^{-1}}(y)$, then for $b \in \{0, 1\}$: 

$$
\color{orange}P^{(i)}(x_b) = P_0^{(i+1)}(q^{(i)}(x_b))+x_bP_1^{(i+1)}(q^{(i)}(x_b))=P_0^{(i+1)}(y)+x_bP_1^{(i+1)}(y)
$$

Since $f^{(i)}(x) = P^{(i)}(x)$ for all $x \in S^{(i)}$, and $f^{(i+1)} = fold(f^{(i)}, r_i’)$, we have:

$$
\color{orange}\begin{align}P^{(i+1)}(y)=f^{(i+1)}(y) &= \begin{bmatrix}
1-r_i'  & r_i'
\end{bmatrix}\begin{bmatrix}
x_1 &-x_0 \\
-1 &1
\end{bmatrix}\begin{bmatrix}
f^{(i)}(x_0)\\
f^{(i)}(x_1)
\end{bmatrix} \\&= \begin{bmatrix}
1-r_i'  & r_i'
\end{bmatrix}\begin{bmatrix}
x_1 &-x_0 \\
-1 &1
\end{bmatrix}\begin{bmatrix}
P^{(i)}(x_0)\\
P^{(i)}(x_1)
\end{bmatrix}\\&=\begin{bmatrix}
1-r_i'  & r_i'
\end{bmatrix}\begin{bmatrix}
x_1 &-x_0 \\
-1 &1
\end{bmatrix}\begin{bmatrix}
1 &x_0 \\
1 &x_1
\end{bmatrix}\begin{bmatrix}
P_0^{(i+1)}(y)\\
P_1^{(i+1)}(y)
\end{bmatrix} \\&=\begin{bmatrix}
1-r_i'  & r_i'
\end{bmatrix}\begin{bmatrix}
x_1-x_0 &0 \\
0 &x_1-x_0
\end{bmatrix}
\begin{bmatrix}
P_0^{(i+1)}(y)\\
P_1^{(i+1)}(y)
\end{bmatrix}\\&=\begin{bmatrix}
1-r_i'  & r_i'
\end{bmatrix}
\begin{bmatrix}
P_0^{(i+1)}(y)\\
P_1^{(i+1)}(y)
\end{bmatrix}\end{align}
$$

($x_1 - x_0 = 1$ according to the definition of the construction of $S^{(i)}(X)$. )

According to such relation and  $P^{(0)}(X) = \sum_{v \in \mathcal{B}_{\ell}} t(v) X_{\{v\}}(X)$, we finally will have $f^{(l)}(X) = P^{(l)}(X)=\sum_{\{v\in \mathcal{B}_\ell\}}t(v)\widetilde{eq}(v,r_0', r_1',...,r_{\ell-1}') =t(r_0’, r_1’,…, r_{\ell-1}’) = c$                       

## *4. Ring Switching*

The goal of this section is to provide a generic ***small-field-to-large-field*** compiler that can reduce an IOPCS over large field into a small field scheme. 

### ***1.Tensor Products of Fields:***

For extension field $L/ K$, define tensor product $A:= L \otimes_K L$, which can be depicted as follows:

![image.png](A%20Note%20on%20Binius-FRI%208af20a731bc843ee91da3963b90612c8/image%201.png)

Let $deg(L/B) = 2^\kappa,$ referring to Section 2.6.2, an $A$-element can be depicted as an array of $2^\kappa \times 2^\kappa K$-elements. Formally, fix a $K$-basis $(\beta_v)_{\{v \in \mathcal{B}_\kappa\} }$ of L, then the set $(\beta_u \otimes \beta_v)_{\{u,v \in \mathcal{B}\kappa\} }$ yields a $K$-basis of $A$. 

Define maps $\varphi_0$ and $\varphi_1$ as $L \otimes \{1\}$ and $\{1\} \otimes L$, respectively. (both $\varphi_0$ and $\varphi_1$ are $L$ —> $A$)

***Definition:***

For extension field $L$ with $K$-basis $(\beta_v)_{\{v \in \mathcal{B}_\kappa\} }$ and each multilinear polynomial $t(X_0, X_1,…, X_{\ell-1}) \in K[X_0, X_1,…, X_{\ell-1}]^{\preceq 1}$, let $\ell’ = \ell - \kappa$, define the ***packed polynomial*** $t'(X_0, X_1,…, X_{\ell'-1})\in L[X_0, X_1,…, X_{\ell'-1}]^{\preceq 1}$, for each $v \in \mathcal{B}_{\ell’}$, $t’(v) = \sum_{u \in \mathcal{B}_\kappa} t(u_0,…u_{\kappa-1}, v_0, …, v_{\ell’-1}) \cdot \beta_u$, thus $t’(X_0,..,X_{\ell’-1}) = \sum_{v \in \mathcal{B}_{\ell’}} t’(v)\widetilde{eq}(X_0,..,X_{\ell’-1},v)$

***Remark:***

Define  $\varphi_1(t’)(X_0, X_1,…, X_{\ell’-1}) \in A[X_0, X_1,…, X_{\ell’-1}]$. Informally, $\varphi_1(t’)$ can be represented as a row vector consisting of the following elements:

![image.png](A%20Note%20on%20Binius-FRI%208af20a731bc843ee91da3963b90612c8/image%202.png)

### ***2.Ring-Switching Protocol:***

A large-field scheme $\Pi’$ over field $L$ is given. And this protocol aims to output a scheme $\Pi$ suitable for small field $K$. 

***1.Setup Phase:***

- $deg(L/K) = 2^\kappa$

***2.Commit Phase:***

***private input:*** $t(X_0, X_1,…, X_{\ell-1}) \in K[X_0, X_1,…, X_{\ell-1}]^{\preceq 1}$

***Step 1***: Compute packed polynomial $t'\in L[X_0, X_1,…, X_{\ell'-1}]^{\preceq 1}$ of $t$;

***Step 2:*** Use $\Pi’.$ Commit to commit $t’$.

***3.Evaluation Phase:***

***Public Input***: evaluation point $\vec{r} \in L^\ell$, $s \in L$,  commitment $[f]$

***Private Input: $t(X_0, X_1,…, X_{\ell-1}) \in K[X_0, X-1,…, X_{\ell-1}]^{\preceq 1}$***

1. P sets $h(X_0, X_1, …, X_{\ell'-1}) = \varphi_1(t’) \cdot \widetilde{eq}(\varphi_0(r_\kappa),...,\varphi_0(r_\ell),X_0,...,X_{\ell'-1})$;
    
    *（Note: here $\varphi_1(t’)$ is an embedding function over tensor field A)*
    
2. P computes $s_0 = \varphi_1(t')(\varphi_0(r_\kappa),...,\varphi_0(r_{\ell-1})).$
3. P —> V: $s_0$
4. V extends $s_0$ to $(s_{0,v})_{v \in \mathcal{B}_\kappa}$ *(Note: since $\varphi_1$ is in $A[X_0,..,X_{\ell’-1}]$, $s_0$ is an A-element)*
5. V checks $s ?= \sum_{v \in\mathcal{B}_\kappa}s_{0,v} \cdot \widetilde{eq}(r_0, r_1,..,r_{\kappa-1},v_0, ..,v_{\kappa-1})$
6. P and V executes the following loop:
    1. for $i \in \{0, …, \ell’-1\}$ do:
        1. P —> V: $h_i(X) = \sum_{w \in \mathcal{B}_{\ell’ - i -1}}h(\varphi_0(r_0'),..,\varphi_0(r_{i-1}'),X,w_0,...,w_{\ell'-i-2})$
        2. V checks $s_i ?= h_i(0) + h_i(1)$. V samples $r_i’$ and sets $s_{i+1} = h_i(\varphi_1(r_i’))$
        3. V —> P: $r_i’$
7. P computes $s’ = t’(r_0’, r_1’,…, r_{\ell’-1}’)$ and sends V $s’.$
8. V checks $s_{\ell’} ?=\varphi_1(s’) \cdot \widetilde{eq} (\varphi_0(r_\kappa),…,\varphi_0(r_{\ell -1}), \varphi_1(r_0’),…, \varphi_1(r_{\ell-1}'))$
9. P and V run $\Pi$. Eval protocol on $([f], s’, r’; t’)$

***4.Completeness Proof:***

***Step 1***: check $s_0$:

For honest prover:

 $s_0 = \varphi_1(t')(\varphi_0(r_\kappa),...,\varphi_0(r_{\ell-1})) = \sum_{w \in \mathcal{B}_{\ell'}}\varphi_1(t')(w)\cdot \widetilde{eq}(\varphi_0(r_\kappa),...,\varphi_0(r_{\ell-1}),w)$

According to the Remark, $\varphi_1(t')(w)$ has the representation of $(t(u_0, u_1,…,u_{\kappa-1}, w_0, …, w_{\ell’-1}))_{u\in \mathcal{B^\kappa}}$

Besides, $\widetilde{eq}(\varphi_0(r_\kappa),...,\varphi_0(r_{\ell-1}),w) = \varphi_0(\widetilde{eq}(r_\kappa, ...,r_{\ell-1},w))$:

***Proof:***

Write $r_i \in L$ as $\sum_{j=0}^{2^\kappa-1}c_{i,j} \beta_j$, where $c_j \in K$, then $\varphi_0(r_i)$ is a column vector of length $2^\kappa$: $\varphi_0(r_i) = (c_{i,0}, c_{i,1},…, c_{i,2^\kappa-1})$.

Then

$$
 \begin{align}\widetilde{eq}(r_\kappa, ...,r_{\ell-1},w) &= \prod_{i=0}^{\ell'-1}(1-r_{\kappa+i})(1-w_i)+r_{\kappa+i}w_i\\&= \prod_{i=0}^{\ell'-1}(\sum_{j=0}^{2^\kappa-1}((d_j-c_{\kappa+i,j})(1-w_i)+c_{\kappa+i,j}w_i)\beta_j)\\&=\sum_{j=0}^{2^\kappa-1}(\prod_{i=0}^{\ell'-1}(d_j-c_{\kappa+i,j})(1-w_i)+c_{\kappa+i,j}w_i))\beta_j\end{align}
$$

where $1=\sum_{j=0}^{2^\kappa-1}d_j \beta_j$, thus $\varphi_0(\widetilde{eq}(r_\kappa, ...,r_{\ell-1},w)) = (\prod_{i=0}^{\ell'-1}(d_j-c_{\kappa+i,j})(1-w_i)+c_{\kappa+i,j}w_i)_{j\in[2^\kappa]}$

For the left side:

$$
 \widetilde{eq}(\varphi_0(r_\kappa),...,\varphi_0(r_{\ell-1}),w) = \prod_{i=0}^{\ell'}(1-\varphi_0(r_{\kappa+i}))(1-w_i)+\varphi_0(r_{\kappa+i})w_i
$$

then the  $j$-th component equals $\prod_{i=0}^{\ell'}(d_j-c_j)(1-w_i)+c_jw_i$

Thus, the column representation $s_{0,v}$ of the A-element $s_0$ is 

$s_{0,v} = \sum_{w \in \mathcal{B}_{\ell'}}\widetilde{eq}(r_\kappa, ...,r_{\ell-1},w)(t(v_0, v_1,…,v_{\kappa-1}, w_0, …, w_{\ell’-1}))=t(v_0,...,v_{\kappa-1},r_\kappa,..,r_{\ell-1})$

And $\sum_{v \in \mathcal{B}_\kappa} s_{0,v} \widetilde{eq}(r_0,r_1,…,r_{\kappa-1},v_0,…,v_{\kappa-1}) = \sum_{v \in \mathcal{B}_\kappa} t(v_0,...,v_{\kappa-1},r_\kappa,..,r_{\ell-1})\widetilde{eq}(r_0,r_1,…,r_{\kappa-1},v_0,…,v_{\kappa-1})=t(\vec{r}) = s$

***Step 2***: Run the sum-check:

Since $h(X_0, X_1, …, X_{\ell'-1}) = \varphi_1(t’) \cdot \widetilde{eq}(\varphi_0(r_\kappa),...,\varphi_0(r_\ell),X_0,...,X_{\ell'-1})$, 

then $\sum_{w \in \mathcal{B}_\ell’} h(w) = \sum_{w \in \mathcal{B}_\ell’} \varphi_1(t’) (w)\cdot \widetilde{eq}(\varphi_0(r_\kappa),...,\varphi_0(r_\ell),w)=\varphi_1(t')(\varphi_0(r_\kappa),...,\varphi_0(r_{\ell-1})) = s_0$

The sumcheck process will reduce to check $s_{\ell’} ?=\varphi_1(t’)(\varphi_1(r_0'),...,\varphi_1(r_{\ell'-1}') \cdot \widetilde{eq}(\varphi_0(r_\kappa),...,\varphi_0(r_\ell),\varphi_1(r_0'),...,\varphi_1(r_{\ell'-1}'))$

$\varphi_1(s’)=\varphi_1(t'(r_0',...,r_{\ell'-1}))$, which is equal to $\varphi_1(t')(r_0',...,r_{\ell'-1})$ :

*(deduced by definition)*

Then $s_{\ell’} = \varphi_1(s’)\cdot \widetilde{eq}(\varphi_0(r_\kappa),...,\varphi_0(r_\ell),\varphi_1(r_0'),...,\varphi_1(r_{\ell'-1}'))$

## *5. IOPCS over Small Field*

The IO-PCS over Small Field is a combination of Section 3 and 4. 

### *1.Setup Phase:*

- variable numbers: $\ell$
- Field $K=\tau_\iota$, $L = \tau_\tau$
- $\kappa = \tau - \iota$; $\ell’ = \ell - \kappa$
- the $L$-basis for  $L[x]^{\prec 2^{\ell'}}$ is: $(X_0(X), X_1(X), …, X_{2^\ell-1}(X))$
- folding factor  $\vartheta$
- repetition parameter $\gamma$
- domains $S^{(0)}, …, S^{(\ell')}$
- polynomials $q^{(0)}, …, q^{(\ell'-1)}$
- RS code Rate $\mathcal{R}$

### ***2.Commit Phase:***

Private Input: $t(X_0, X_1, …,, X_{\ell -1}) \in K[X_0, X_1,…, X_{\ell-1}]^{\preceq 1}$

***Step 1:*** P computes packed polynomial $t'\in L[X_0, X_1,…, X_{\ell'-1}]^{\preceq 1}$ of $t$;

***Step 2:*** P computes $\{t'(v)\}_{v \in \mathcal{B}_{\ell'}}$

***Step 3:*** P construct the *univariate* polynomial $P(x) = \sum_{v \in \mathcal{B}_{\ell'}} t'(v) X_{\{v\}}(X)$ 

***Step 4:*** P computes the R-S codeword $f: S^{(0)} \longrightarrow  L$ defined by $f: x \longrightarrow  P(x)$

***Step 5:*** Commits to $f$ as $[f]$

### ***3.Evaluation Phase:***

***Claim***: P claims that $t(\vec{r}) = s$.

***Public Input***: $[f], s \in L$, and $\vec{r} \in L^{\ell}$

***Private Input:*** $t(X_0, X_1, …,, X_{\ell -1}) \in K[X_0, X_1,…, X_{\ell-1}]^{\preceq 1}$

1. P construct $h(X_0, X_1, …,, X_{\ell' -1}) = t'(X_0, X_1, …,, X_{\ell' -1})\widetilde{eq}(\varphi_0(r_\kappa),...,\varphi_0(r_\ell),X_0,...,X_{\ell'-1})$
2. P computes $s_0 = t'(\varphi_0(r_\kappa),...,\varphi_0(r_{\ell-1}))$
3. P —> V: $s_0$
4. V extends $s_0$ to $(s_{0,v})_{v \in \mathcal{B}_\kappa}$;
5. V checks $s ?= \sum_{v \in\mathcal{B}_\kappa}s_{0,v} \cdot \widetilde{eq}(r_0, r_1,..,r_{\kappa-1},v_0, ..,v_{\kappa-1})$
6. Set $f^{(0)} = f$.
    
    for $i \in \{0, …, \ell' -1\}$ do:
    
    1. ***P —> V***: the univariate polynomial $h_i(x) = \sum_{v\in \mathcal{B}_{\ell'-i-1}}h(\varphi_1(r_0’),…,\varphi_1(r_{i-1}’), X, v_0, v_1,…,v_{\ell' -i-2})$
    2. V checks $s_i ?= h_i(0) + h_i(1)$, samples $r_i’\in L,$ and set $h_i(\varphi_1(r_i’)) = s_{i+1}$
    3. ***V —> P***: $r_i’$
    4. P defines $f^{(i+1)} = fold(f^{(i)}, r_i’)$
    5. if $i+1 = \ell'$: $P —> V: c = f^{(\ell')}(0,..0)$ 
    6. else if $\vartheta | i+1$ P commits to $f^{(i+1)}$, and send $[f^{(i+1)}]$ to V
7. V checks $s_{\ell'} ?= \varphi_1(c) \cdot\widetilde{eq}(\varphi_0(r_\kappa),...,\varphi_0(r_\ell),\varphi_1(r_0’),…,\varphi_1(r_{\ell'-1}’))$
8. V executes the following querying procedure: 
    
    for $\gamma$ repetitions do 
    
    1. V samples v $\in \mathcal{B}_{\ell' + \mathcal{R}}$
    2. for $i \in \{0, \vartheta, …, \ell' - \vartheta\}$ do:
        1. for each $u \in \mathcal{B}_\vartheta$, V queries $[f^{(i)}](u_0, u_1,…,u_{\vartheta-1}, v_{i+\vartheta}, …, v_{\ell'+\mathcal{R}-1})$
        2. if $i > 0$, V additionally checks $c_i ?= f^{(i)}(v_i, …, v_{\ell' + \mathcal{R}-1})$
        3. V defines $c_{i+\vartheta}:=fold(f^{(i)}, r_i’, …, r_{i+\vartheta-1})(v_{i+\vartheta}, …, v_{\ell' + \mathcal{R} -1})$
    3. V checks $c_{\ell'} ? = c$