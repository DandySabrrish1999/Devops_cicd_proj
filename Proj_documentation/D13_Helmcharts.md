## Helm 
- Helm is a pcakage manager for kubernetes
- A chart is a package of pre-configured kubernetes resources
- A repository is a group of published charts which can be made availabe to others
- Helm is used to repeat the tasks and applications
- Helm should be installed on jenkins slave or build server/node

#### Quick difference between docker,docker-hub & Helm charts
**Docker and Docker Hub:**
- Imagine you bake a cake. The cake is your app.
- Docker is like the cake box that wraps up your cake so you can transport it anywhere without it falling apart. This box makes sure the cake stays in perfect condition, whether it's on your table or someone else's.
- Docker Hub is like an online bakery store where you can upload your cake (inside the Docker box) so other people can download it and enjoy the same cake. They don’t need to bake it from scratch because the whole cake (your app) is ready.

**Helm:**
- Now, you don’t just need a cake; you want to set up a full party (like a big app with many pieces).

- Helm is like a party planner that organizes everything for you: cake (your Docker app), decorations, drinks, plates, and music (other configurations and services). You don’t have to do all these things one by one—Helm plans the whole party and sets it up.

- So, while Docker helps package the cake, Helm helps plan the whole party by organizing everything (including the cake) and setting it up in Kubernetes.
