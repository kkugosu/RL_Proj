implementation






## TRPO

when distributional shift occered while updating, parameter change but performance does not increasing. to deal with this problem, trpo set lower bound of performance while updating parameter. in this paper, auther change $ \sum_s \rho_\tilde{\pi} (s) \sum_a \tilde{\pi} (a|s) A_\pi (s,a) $ to $ \sum_s \rho_\pi (s) \sum_a \tilde{\pi} (a|s) A_\pi (s,a) $ because we can't use trajectary of updated policy. even though we make policy close to order one, we have to deal with derivirative form of that formula which is crazy. so we change formula and add $ -CD_{KL}^{max}(\pi_i , \pi)$ penalty term which allow to change formula.

auther use the fact that $ \pi_{i+1} = \underset{\pi}{argmax} ( L_{\pi i} (\pi) - CD_{KL}^{max}(\pi_i , \pi)) $ is one of form of lagrange mulifiplier formula, they manipulate term C to converge more faster with take a risk of little distributional shift.

this is last formula 

$$ \underset{\theta}{maximize} (\nabla_\theta L_{\theta old} (\theta) \cdot (\theta - \theta_{old}) )$$


$$ subject \ to \ {1 \over 2}(\theta_{old} - \theta)^T A (\theta_{old} - \theta) \leqq \delta $$

A equals fisher matrix which is hessian of kld. 

when solve that formula, we get updating process like this $ \theta_{new} - \theta_{old} = {1 \over \lambda} A (\theta_{old})^{-1} \nabla_\theta L (\theta)$

$$ 1.\ take \ action \ a \sim \pi_\theta (a|s), store (s, a, s', r)\ in \ memory $$

$$ 2.\ sample \ a \ batch \ (s_i, a_i, r_i, s_i')\ from \ memory, \ load \ \phi' \ from \ \phi$$

$$ 3.\ \phi \leftarrow \phi - \alpha \Sigma_i {dQ \over d\phi} (s_i , a_i) (Q_\phi (s_i , a_i) - (r(s_i , a_i) + \gamma Q_{\phi'}(s_i',a_i'))) $$

$$ 4. update \ \theta $$

## PPO

we don't have to use Q function approximization. because V function and policy function share parameter $ \theta $ and we update these functions simultaneously, we always refill buffer with new policy.

$$ L^{CLIP} (\theta) = \hat{E_t} (min (r_t (\theta) \hat{A_t} , clip (r_t (\theta), 1 - \epsilon, 1 + \epsilon)\hat{A_t})$$

$$ L_t^{CLIP + VF + S} (\theta) = \hat{E_t} (L_t^{CLIP} (\theta) - c_1L_t^{VF}(\theta) + c_2S [\pi_\theta] (s_t)) $$
