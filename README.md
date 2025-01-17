using System.Collections.Generic;
using UnityEngine;

public class UsesPatrolPoints : MonoBehaviour
{
    public List<GameObject> PatrolPoints;

    public List<GameObject> GetPatrolPoints()
    {
        return PatrolPoints;
    }
}




using System.Collections.Generic;
using UnityEngine;

/// <summary>
/// Instructions: Attach to an enemy. Drag some Patrol Nodes into inspector.
/// </summary>
public class PatrolSimple : MonoBehaviour
{
    public List<Transform> patrolPoints;
    public int currentPoint = 0;
    public float enemySpeed = 5;

    void Update()
    {
        if (patrolPoints == null || patrolPoints.Count == 0) return;

        if (!patrolPoints[currentPoint]) patrolPoints.RemoveAt(currentPoint);

        MoveTowardPatrolPoint();

        if (IsNearPatrolPoint()) currentPoint++;

        if (currentPoint == patrolPoints.Count) currentPoint = 0;
    }

    private void MoveTowardPatrolPoint()
    {
        transform.position = Vector3.MoveTowards(
            transform.position, 
            patrolPoints[currentPoint].position, 
            enemySpeed * Time.deltaTime);
    }

    bool IsNearPatrolPoint(float threshold = 0.1f)
    {
        var p1 = transform.position;
        var p2 = patrolPoints[currentPoint].position;
        return IsNear(p1, p2, threshold);
    }

    bool IsNear(Vector3 p1, Vector3 p2, float threshold = 0.1f)
    {
        return (IsNearX(p1, p2, threshold) && IsNearY(p1, p2, threshold) && IsNearZ(p1, p2, threshold));
    }

    bool IsNearX(Vector3 p1, Vector3 p2, float threshold = 0.1f)
    {
        return Mathf.Abs(p1.x - p2.x) < threshold;
    }

    bool IsNearY(Vector3 p1, Vector3 p2, float threshold = 0.1f)
    {
        return Mathf.Abs(p1.y - p2.y) < threshold;
    }

    bool IsNearZ(Vector3 p1, Vector3 p2, float threshold = 0.1f)
    {
        return Mathf.Abs(p1.z - p2.z) < threshold;
    }
}
